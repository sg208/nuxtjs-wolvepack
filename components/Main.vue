<template>
  <main class="container">
    <b-tabs class="gap" content-class="mt-3">
      <TabContent v-for="tab in tabs" :key="tab.id" :tabdetail="tab" />
    </b-tabs>
  </main>
</template>

<script>
import { mapState } from 'vuex'

export default {
  computed: {
    ...mapState([
      'tabs'
    ])
  }
}
</script>

<style lang="scss" scoped>
.gap {
    margin-top: 2.5rem;
}
</style>
