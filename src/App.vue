<template>
  <div>
    <PokeHome />
  </div>
</template>

<script>
import PokeHome from "./components/PokeHome.vue"

export default {
  name: "App",

  components: { PokeHome },
}
</script>

<style lang="scss">
#app {
  padding: 0 200px;
  color: white;
  background: linear-gradient(
      to bottom,
      rgba(10, 10, 10, 1) 0.2%,
      rgb(255, 238, 0),
      rgba(255, 239, 19, 0.637)
    )
    no-repeat center center fixed !important;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover !important;
  background-position: center;
  min-height: 100vh;
}
</style>
