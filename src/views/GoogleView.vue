<template>
<div>TEst</div>
</template>

<script>
import speech from '@google-cloud/speech';
const client = new speech.SpeechClient();

import MicRecorder from 'mic-recorder-to-mp3';
// New instance
const recorder = new MicRecorder({
  bitRate: 128
});

// Start recording. Browser will request permission to use your microphone.
recorder.start().then(() => {
  // something else
}).catch((e) => {
  console.error(e);
});

// Once you are done singing your best song, stop and get the mp3.
recorder
    .stop()
    .getMp3().then(([buffer, blob]) => {
  // do what ever you want with buffer and blob
  // Example: Create a mp3 file and play
  const file = new File(buffer, 'me-at-thevoice.mp3', {
    type: blob.type,
    lastModified: Date.now()
  });

  const player = new Audio(URL.createObjectURL(file));
  player.play();

}).catch((e) => {
  alert('We could not retrieve your message');
  console.log(e);
});

export default {
  name: "HomeView",
  data: () => ({
  }),
  mounted() {
    this.quickstart();
  },
  methods: {
    async quickstart() {
      // The path to the remote LINEAR16 file
      const gcsUri = 'gs://cloud-samples-data/speech/brooklyn_bridge.raw';

      // The audio file's encoding, sample rate in hertz, and BCP-47 language code
      const audio = {
        uri: gcsUri,
      };
      const config = {
        encoding: 'LINEAR16',
        sampleRateHertz: 16000,
        languageCode: 'en-US',
      };
      const request = {
        audio: audio,
        config: config,
      };

      // Detects speech in the audio file
      const [response] = await client.recognize(request);
      const transcription = response.results
          .map(result => result.alternatives[0].transcript)
          .join('\n');
      console.log(`Transcription: ${transcription}`);
    }
  }
}
</script>

<style scoped>

</style>
