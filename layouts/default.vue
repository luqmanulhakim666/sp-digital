<template>
  <v-app id="inspire">
    <Navbar />
    <v-main class="o-app">
      <Nuxt />
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: () => ({ drawer: null }),
};
</script>

<style lang="scss" scoped>
.o-app {
  overflow: hidden;
  height: 100vh;
  @media (max-width: 900px) {
    overflow-y: auto;
  }
}
</style>
