<template>
  <footer>
    <div class="container">
      Crafted with love by <a href="https://github.com/sg208" target="_blank">github.com/sg208</a> | {{ currentYear }}
    </div>
  </footer>
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
footer {
  border-top: 1px solid #ccc;
  height: 10rem;
  margin-top: 2rem;
  background-color: #efefef;
  color: darkgray;

  .container {
    margin-top: 2rem;
  }
}
</style>
