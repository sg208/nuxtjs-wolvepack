<template>
  <div>
    <b-alert variant="danger" :show="dismissCountDown" :dismissible="content.dismissible">
      {{ content.text }}
    </b-alert>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/require-prop-types
  props: ['content'],
  data () {
    return {
      dismissCountDown: this.content.dismissible ? 8 : 0
    }
  }
}
</script>
