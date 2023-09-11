<template>
  <v-layout>
    <v-app-bar color="blue" prominent>
      <v-app-bar-nav-icon
        variant="text"
        @click.stop="drawer = !drawer"
      ></v-app-bar-nav-icon>
      <h3
        @click="$router.push('/')"
        style="
          font-size: x-large;
          font-weight: 500;
          text-transform: uppercase;
          cursor: pointer;
        "
      >
        todo leader
      </h3>
      <v-spacer></v-spacer>

      <!--  -->
      <v-btn>
        <h5>Sign Out</h5>
        <v-icon size="large">mdi-exit-to-app</v-icon>
      </v-btn>
    </v-app-bar>

    <!--  -->

    <v-container>
      <v-snackbar
        class="d-flex align-center justify-space-between"
        location="top"
        :timeout="2000"
        v-model="snackbar"
      >
        <span>Project Is Added</span>
        <v-spacer></v-spacer>
        <v-btn
          flat
          color="white"
          variant="text"
          class="text-end w-100"
          @click="snackbar = false"
          >Close</v-btn
        >
      </v-snackbar>
    </v-container>
    <!--  -->

    <v-navigation-drawer v-model="drawer" location="left" temporary>
      <v-row class="my-5">
        <v-col class="text-center">
          <v-avatar variant="outlined" size="160" rounded="50">
            <v-img
              src="https://us.123rf.com/450wm/photoart23d/photoart23d1705/photoart23d170500084/77462832-rojo-de-la-letra-a-3d-aislado-en-blanco-representaci%C3%B3n-3d.jpg"
            ></v-img>
          </v-avatar>
          <h3 class="text-blue mt-2">Abdallah</h3>
        </v-col>
      </v-row>
      <Popap @projectAdded="snackbar = true" class="my-5" />
      <v-list
        style="
          display: flex;
          align-items: center;
          justify-content: space-between;
          gap: 10px;
          text-align: center;
        "
        v-for="item in items"
        :key="item.route"
      >
        <router-link class="route" :to="item.route">
          <v-icon class="mx-5">mdi-{{ item.icon }}</v-icon>
          {{ item.title }}</router-link
        >
      </v-list>
    </v-navigation-drawer>
  </v-layout>
</template>

<script>
import Popap from "./Popap.vue";
export default {
  data: () => ({
    drawer: false,
    group: null,
    snackbar: false,
    items: [
      {
        title: "Dashboard",
        icon: "view-dashboard",
        route: "/",
      },
      {
        title: "Projects",
        icon: "folder",
        route: "/project",
      },
      {
        title: "Team",
        icon: "account",
        route: "/team",
      },
    ],
  }),

  watch: {
    group() {
      this.drawer = false;
    },
  },
  components: {
    Popap,
  },
};
</script>

<style lang="scss" scoped>
.route {
  text-decoration: none;
  width: 100%;
  text-align: left;
  display: flex;
  align-items: center;
}
a {
  text-decoration: none;
  width: 100%;
  text-align: left;
  padding: 18px;
  border-radius: 10px;
  color: dodgerblue;
  text-align: center;
}
</style>
