<template>
  <v-app>
    <v-app-bar app color="primary" dark>Title</v-app-bar>

    <v-main>
      <WatsonView></WatsonView>
    </v-main>
  </v-app>
</template>

<script>
import WatsonView from "./views/WatsonView";

export default {
  name: 'App',

  components: {
    WatsonView,
  },

  data: () => ({
    //
  }),
};
</script>
