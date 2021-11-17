<template>
  <div>
    <Header />
    <Main />
    <Footer />
  </div>
</template>

<script>
import Header from '~/components/Header.vue'
export default { components: { Header } }
</script>
<style>
body {
  overflow: scroll;
}
</style>
