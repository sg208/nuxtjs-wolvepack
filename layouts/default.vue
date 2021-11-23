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

<style lang="scss">
body {
  overflow: scroll;
}
header {
  padding: 2rem 0;
  background-color: #efefef;
  color: white;
  margin-bottom: 2rem;
  border-bottom: .5rem solid #80ED99;
}
footer {
  border-top: 1px solid #ccc;
  height: 10rem;
  margin-top: 2rem;
  background-color: #efefef;
  color: darkgray;
}
</style>
