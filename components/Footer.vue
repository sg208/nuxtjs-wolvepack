<template>
  <div class="container">
    Crafted with love by <a href="https://github.com/sg208" target="_blank">github.com/sg208</a> | All rights reserved | {{ currentYear }} | <NuxtLink to="/privacy">
      Privacy
    </NuxtLink>
  </div>
</template>

<script>
export default {
  data () {
    return {
      currentYear: new Date().getFullYear()
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
    margin-top: 2rem;
  }
</style>
