<template>
  <!-- <div class="row">
    <div v-if="isDoctor" class="col-6">
      <CommentForm @comment-submited="addComment" />
    </div> -->
  <div class="col-12">
    <CommentList
      v-if="GStore.comments"
      :comments="GStore.patient.doctorRec"
      :commentsHistory="GStore.patient"
    />
  </div>
  <!-- </div> -->
</template>
<script>
//import CommentForm from '@/components/CommentForm.vue'
import CommentList from '@/components/CommentList.vue'
//import AuthService from '@/service/AuthService'
//import GStore from '@/store'
//import CommentService from '@/service/CommentService.js'
export default {
  inject: ['GStore'],
  components: {
    //CommentForm
    CommentList
  },
  data() {
    return {
      newComment: null
    }
  }
  // methods: {
  //   addComment(comment) {
  //     console.log(comment)
  //     GStore.comments.push(comment)
  //     // GStore.patient.doctorRec = GStore.comments.filter(
  //     //   (patient) => GStore.patient.id == patient.patient_id
  //     // )
  //     CommentService.addComment(GStore.patient.id, comment)
  //   }
  // },
  // computed: {
  //   isDoctor() {
  //     return AuthService.hasRoles('ROLE_DOCTOR')
  //   }
  // }
}
</script>
