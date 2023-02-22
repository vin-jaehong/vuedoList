<template>
  <!--헤더-->
  <Header></Header>
  <!--컨텐츠 영역-->
  <Container>
    <!--여기서 라우팅-->
    <Main></Main>
  </Container>
</template>

<script setup>
  import Container from './components/Container.vue';
  import Header from './components/Header.vue';
  import Main from './pages/Main.vue';
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
</style>
