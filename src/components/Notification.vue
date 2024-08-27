<template>
  <div v-if="message" :class="['notification', type]">
    {{ message }}
  </div>
</template>

<script>
export default {
  props: {
    message: String,
    type: String,
  },
};
</script>

<style>
.notification {
  position: fixed;
  bottom: 10px;
  right: 10px;
  padding: 10px;
  border-radius: 5px;
  color: #fff;
  background-color: #444;
}
.notification.success {
  background-color: green;
}
.notification.error {
  background-color: red;
}
</style>
