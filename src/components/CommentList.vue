<template>
  <div class="comment-container">
    <h4>Doctor Comment</h4>
    <div
      id="comment"
      v-for="commentHistory in commentsHistory.commentList"
      :key="commentHistory"
    >
      <p>-----------------------------------</p>
      <p>
        Patient: {{ commentHistory.patient_name }}
        {{ commentHistory.patient_surname }}
      </p>
      <p>Doctor: {{ commentHistory.name }}</p>
      <p>Date: {{ commentHistory.date }}</p>
      <p>Comment: {{ commentHistory.comment }}</p>
    </div>
  </div>
</template>
<script>
export default {
  props: ['comments', 'commentsHistory']
}
</script>

<style scoped>
.comment-container {
  width: 425px;
  padding: 20px;
  font-size: 18px;
  text-align: left;
  margin: auto;
  margin-top: 40px;
  border-radius: 10px;
  background: #eff9fe;
  -webkit-box-shadow: 0px 2px 20px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 20px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 20px -12px rgba(0, 0, 0, 0.57);
  border: 2px solid #d8d8d8;
}
</style>
