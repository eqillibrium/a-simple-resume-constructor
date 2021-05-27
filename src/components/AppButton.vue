<template>
  <button
      class="btn"
      :class="color"
      :disabled="contentLength"
      @click="$emit('action')"
  >
    <slot></slot>
  </button>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      required: false,
      default: ''
    },
    contentLength: {
      type: Boolean,
      required: false
    }
  },
  emits: {
    'action': null
  }
}
</script>

<style scoped>

</style>