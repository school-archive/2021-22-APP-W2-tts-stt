<template>
<div>hallo</div>
</template>

<script>
import axios from 'axios';

export default {
  name: "WatsonView",
  mounted() {
    const url = 'https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac';
    axios.get()
  }
}
</script>

<style scoped>

</style>
