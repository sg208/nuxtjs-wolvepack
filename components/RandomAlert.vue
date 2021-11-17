<template>
  <div>
    <b-alert variant="danger" show>
      {{ errortext }}
    </b-alert>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/require-prop-types
  props: ['errortext']
}
</script>
