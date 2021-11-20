<template>
  <b-modal :id="content.id" centered ok-only :title="`Update ${content.title} data`">
    <p class="my-4">
      You can update <b>{{ content.name }}</b> data soon, just not today!
    </p>
  </b-modal>
</template>

<script>
export default {
  // eslint-disable-next-line vue/require-prop-types
  props: ['content']
}
</script>

<style lang="scss" scoped>

</style>
