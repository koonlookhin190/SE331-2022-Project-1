<template>
  <div class="col-md-12">
    <div clas="card card-container">
      <img
        id="profile-img"
        src="//ssl.gstatic.com/accounts/ui/avatar_2x.png"
        class="profile-img-card"
      />
      <Form @submit="handleRegiter" :validation-schema="schema">
        <div v-if="!successful">
          <div class="form-group">
            <label for="firstname">First Name </label>
            <Field name="firstname" type="firstname" class="form-control" />
            <ErrorMessage name="firstname" class="error-feedback" />
          </div>
          <div class="form-group">
            <label for="lastname">Last Name</label>
            <Field name="lastname" type="lastname" class="form-control" />
            <ErrorMessage name="lastname" class="error-feedback" />
          </div>
          <div class="form-group">
            <label for="gender">Gender</label>
            <Field name="gender" type="gender" class="form-control" />
            <ErrorMessage name="gender" class="error-feedback" />
          </div>
          <div class="form-group">
            <label for="age">Age</label>
            <Field name="age" type="age" class="form-control" />
            <ErrorMessage name="age" class="error-feedback" />
          </div>
          <div class="form-group">
            <label for="hometown">Hometown</label>
            <Field name="hometown" type="hometown" class="form-control" />
            <ErrorMessage name="hometown" class="error-feedback" />
          </div>
          <div class="form-group">
            <label for="username">Username</label>
            <Field name="username" type="text" class="form-control" />
            <ErrorMessage name="username" class="error-feedback" />
          </div>
          <div class="form-group">
            <label for="email">Email</label>
            <Field name="email" type="email" class="form-control" />
            <ErrorMessage name="email" class="error-feedback" />
          </div>
          <div class="form-group">
            <label for="password">Password</label>
            <Field name="password" type="password" class="form-control" />
            <ErrorMessage name="password" class="error-feedback" />
          </div>
          <UploadImages @changed="handleImages" />
          <div class="form-group">
            <button class="btn btn-primary btn-block" :disabled="loading">
              <span
                v-show="loading"
                class="spinner-border spinner-border-sm"
              ></span>
              Sign Up
            </button>
          </div>
        </div>
      </Form>
      <div
        v-if="message"
        class="alert"
        :class="successful ? 'alert-success' : 'alert-danger'"
      >
        {{ message }}
      </div>
    </div>
  </div>
</template>
<script>
import UploadImages from 'vue-upload-drop-images'
import AuthService from '@/service/AuthService.js'
import PatientService from '@/service/PatientService'
import { Form, Field, ErrorMessage } from 'vee-validate'
import * as yup from 'yup'
export default {
  name: 'RegisterView',
  components: {
    Form,
    Field,
    ErrorMessage,
    UploadImages
  },
  //eslint-disable-next-line
  inject: ['GStore'],
  data() {
    const schema = yup.object().shape({
      firstname: yup
        .string()
        .required('First name is required!')
        .min(3, 'Must be at least 3 charaacter!')
        .max(20, 'Must be maximum 20 characters!'),
      lastname: yup
        .string()
        .required('Last name is required!')
        .min(3, 'Must be at least 3 charaacter!')
        .max(20, 'Must be maximum 20 characters!'),
      username: yup
        .string()
        .required('Username is required!')
        .min(3, 'Must be at least 3 charaacter!')
        .max(20, 'Must be maximum 20 characters!'),
      email: yup
        .string()
        .required('Email is required!')
        .email('Email is invalid!')
        .max(50, 'Must be maxi,u, 50 characters!'),
      password: yup
        .string()
        .required('Password is required!')
        .min(6, 'Must be at least 6 characters!')
        .max(40, 'Must be maximum 40 characters!'),
      age: yup
        .string()
        .min(1, 'Must be at least 1 characters!')
        .max(3, 'Must be maximum 3 characters!'),
      hometown: yup
        .string()
        .required('Password is required!')
        .min(6, 'Must be at least 6 characters!')
        .max(40, 'Must be maximum 40 characters!'),
      gender: yup.string()
    })
    return {
      successful: false,
      loading: false,
      message: '',
      schema,
      files: []
    }
  },
  mounted() {
    if (this.GStore.currentUser) {
      this.$router.push('/')
    }
  },
  methods: {
    // eslint-disable-next-line
    handleRegiter(user) {
      Promise.all(
        this.files.map((file) => {
          return PatientService.uploadFile(file)
        })
      ).then((response) => {
        console.log(response)
        console.log(response.map((r) => r.data))
        user.image = response.map((r) => r.data).toString()
        AuthService.register(user)
          .then(() => {
            this.$router.push({ path: '/login' })
          })
          .catch(() => {
            this.message = 'could not register'
          })
        this.message = ''
        this.successful = false
        this.loading = true
      })
    },
    handleImages(files) {
      console.log('called handle images')
      this.files = files
    }
  }
}
</script>
<style scoped>
label {
  display: block;
  margin-top: 10px;
}

.card-container.card {
  max-width: 350px !important;
  padding: 40px 40px;
}

.card {
  background-color: #f7f7f7;
  padding: 20px 25px 30px;
  margin: 0 auto 25px;
  margin-top: 50px;
  -moz-border-radius: 2px;
  -webkit-border-radius: 2px;
  border-radius: 2px;
  -moz-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  -webkit-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
}

.profile-img-card {
  width: 96px;
  height: 96px;
  margin: 0 auto 10px;
  display: block;
  -moz-border-radius: 50%;
  -webkit-border-radius: 50%;
  border-radius: 50%;
}

.error-feedback {
  color: red;
}
</style>
