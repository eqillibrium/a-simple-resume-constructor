<template>
  <p>
    {{ content }}
  </p>
</template>

<script>
export default {
  data () {
    return {
      content: this.$attrs.content
    }
  }
}
</script>

<style scoped>

</style>