<template>
  <div>
    <b-alert variant="danger" :show="dismissCountDown" dismissible>
      {{ errortext }}
    </b-alert>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/require-prop-types
  props: ['errortext'],
  data () {
    return {
      dismissCountDown: 8
    }
  }
}
</script>
