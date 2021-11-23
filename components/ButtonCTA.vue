<template>
  <b-button :variant="buttonVariant" :size="buttonSize" :to="buttonTo" :class="buttonClass">
    {{ content.text }}
  </b-button>
</template>
<script>
export default {
  props: {
    content: {
      type: Object,
      default: () => ({
        variant: String,
        size: String,
        to: String,
        class: String,
        text: String
      })
    }
  },
  data () {
    return {
      buttonVariant: this.content.variant || 'outline-secondary',
      buttonSize: this.content.size || 'lg',
      buttonTo: this.content.to || '/',
      buttonClass: this.content.class || 'gap-down'
    }
  }
}
</script>
