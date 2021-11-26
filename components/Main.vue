<template>
  <div>
    <b-jumbotron>
      <div class="container">
        <h1>
          The wolves packs collection
        </h1>
        <p>Focus on your core task, not keep managing your data.</p>
      </div>
    </b-jumbotron>
    <div class="container">
      <b-tabs class="gap" content-class="mt-3" lazy>
        <TabContent v-for="tab in tabs" :key="tab.id" :tabdetail="tab" />
      </b-tabs>
    </div>
  </div>
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
.jumbotron {
  background: linear-gradient(119deg, rgba(204,204,204,1) 21%, rgba(239,239,239,1) 80%);
  border-radius: 0px;
}
</style>
