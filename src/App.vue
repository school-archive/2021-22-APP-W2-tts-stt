<template>
  <v-app>
    <v-app-bar app color="primary" dark>Title</v-app-bar>

    <v-main>
      <WebkitView></WebkitView>
    </v-main>
  </v-app>
</template>

<script>
import WebkitView from "@/views/WebkitView";

export default {
  name: 'App',

  components: {
    WebkitView,
  },

  data: () => ({
    //
  }),
};
</script>
