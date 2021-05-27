<template>
  <div class="avatar">
    <img :src="content">
  </div>
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