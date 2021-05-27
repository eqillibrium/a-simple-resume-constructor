<template>
  <li class="list-item">
    <div>
      <p><strong>{{ email }}</strong></p>
      <small>{{ body }}</small>
    </div>
  </li>
</template>

<script>
export default {
  props: {
    email: {
      type: String,
      required: false
    },
    body: {
      type: String,
      required: false
    }
  }
}
</script>

<style scoped>

</style>