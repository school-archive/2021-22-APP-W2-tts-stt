<template>
  <v-app>
    <v-app-bar app color="primary" dark>Title</v-app-bar>

    <v-main>
      <HomeView></HomeView>
    </v-main>
  </v-app>
</template>

<script>
import HomeView from "./views/GoogleView";

export default {
  name: 'App',

  components: {
    HomeView,
  },

  data: () => ({
    //
  }),
};
</script>
