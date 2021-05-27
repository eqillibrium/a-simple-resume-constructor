<template>
  <div class="alert">
    <h3>{{ message }}</h3>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      required: false
    }
  }
}
</script>

<style scoped>

</style>