<template>
  <div>
    <h1>{{ content }}</h1>
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