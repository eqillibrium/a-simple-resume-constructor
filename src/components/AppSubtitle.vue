<template>
  <h2>{{ content }}</h2>
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