<template>
  <!-- <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav> --><NavBar class="my-10" />
  <router-view />
</template>

<script>
import NavBar from "@/components/NavBar.vue";
export default {
  components: {
    NavBar,
  },
};
</script>

<style lang="scss">
#app {
  overflow-x: hidden;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #fff;
      background-color: rgba(0, 0, 0, 0.3);
      padding: 5px;
      border-radius: 5px;
    }
  }
}
</style>
