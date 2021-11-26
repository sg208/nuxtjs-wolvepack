<template>
  <div>
    <header role="banner">
      <Header />
    </header>
    <Nuxt role="main" keep-alive />
    <footer role="contentinfo">
      <Footer />
    </footer>
  </div>
</template>

<style lang="scss" scoped>
header {
  margin-bottom: 2rem;
}
</style>
