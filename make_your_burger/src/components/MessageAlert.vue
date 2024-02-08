<template>
  <div class="message-container">
    <p>{{ msg }}</p>
  </div>
</template>

<script>
export default {
  name: "MessageAlert",
  props: {
    msg: String,
  },
};
</script>

<style scoped>
.message-container {
  color: #004085;
  background-color: #cce5ff;
  border: 2px solid #bbdaff;
  padding: 10px;
  max-width: 400px;
  margin: 30px auto;
}
</style>
