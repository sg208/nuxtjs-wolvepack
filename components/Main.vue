<template>
  <main class="container">
    <b-tabs class="gap" content-class="mt-3">
      <TabContent v-for="tab in tabs" :key="tab.id" :tabdetail="tab" keep-alive />
    </b-tabs>
  </main>
</template>

<script>
export default {
  data () {
    return {
      tabs: [
        {
          id: 'wolve',
          text: {
            heading: 'Wolves',
            singular: 'wolve',
            plural: 'wolves'
          }
        },
        {
          id: 'pack',
          text: {
            heading: 'Packs',
            singular: 'pack',
            plural: 'packs'
          }
        }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
.gap {
    margin-top: 2.5rem;
}
</style>
