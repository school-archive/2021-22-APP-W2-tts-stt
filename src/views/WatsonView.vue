<template>
<div>hallo</div>
</template>

<script>
// import axios from 'axios';
import SpeechToTextV1 from 'ibm-watson/speech-to-text/v1';
import { IamAuthenticator } from 'ibm-watson/auth';

export default {
  name: "WatsonView",
  async mounted() {
    const apikey = 'y4Zf5zNBdMkYObxOl_m2wX6STm_EMRscvXrFsxU4breT';
    // const instance = 'https://api.eu-gb.speech-to-text.watson.cloud.ibm.com/instances/6aa28f9a-858a-4a4b-86ed-0786877d4cfe';
    const file = 'https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac';
    // const res = await axios.post(
    //   instance + '/v1/recognize ',
    //   {
    //     headers: { 'Content-Type': 'audio/flac' },
    //     data: file
    //   },
    //   {
    //     auth: { username: 'apikey', password: apikey }
    //   })
    // .catch(e => console.log(e))
    // console.log(res.data)

    const speechToText = new SpeechToTextV1({
      authenticator: new IamAuthenticator({
        apikey: apikey,
      }),
      serviceUrl: 'https://api.us-east.speech-to-text.watson.cloud.ibm.com',
    });

    const params = {
      // From file
      audio: file,
      contentType: 'audio/flac'
    };

    speechToText.recognize(params)
        .then(response => {
          console.log(JSON.stringify(response.result, null, 2));
        })
        .catch(err => {
          console.log(err);
        });
  }
}
</script>

<style scoped>

</style>
