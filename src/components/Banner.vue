<template>
    <div class="banner">
        <img class="logo-pokedex" :src="logo" alt="logo-pokedex" />
    </div>
</template>

<script>
import logo from "../assets/pokedex.png"

export default {
  name: 'BannerComponent',
  data: function() {
      return {
          logo: logo
      }
  }
}
</script>

<style scoped>
.logo-pokedex {
    padding:2%;
}
</style>
