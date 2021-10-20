<template>
  <v-app>
    <v-app-bar app color="primary" dark><v-icon class="mr-3">
      mdi-translate</v-icon>
      <v-app-bar-title>Speech to Text - Text to Speech</v-app-bar-title>
    </v-app-bar>

    <v-main id="mains">
      <v-container fluid>
        <WebkitView></WebkitView>
      </v-container>
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
  }),
};
</script>

<style>
#main {
  display: grid;
  place-items: center;
}
</style>
