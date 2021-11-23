<template>
  <div>
    <b-alert variant="danger" :show="dismissCountDown" :dismissible="content.dismissible || false">
      {{ content.text }}
    </b-alert>
  </div>
</template>

<script>
export default {
  props: {
    content: {
      type: Object,
      default: () => ({
        dismissible: Number,
        text: String
      })
    }
  },
  data () {
    return {
      dismissCountDown: this.content.dismissible ? 5 : 0
    }
  }
}
</script>
