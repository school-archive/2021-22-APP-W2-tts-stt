<template>
<div>TEst</div>
</template>

<script>
import speech from '@google-cloud/speech';
import { auth } from "google-auth-library"
process.versions = 1;

const authClient = auth.fromJSON({
  "type": "service_account",
  "project_id": "capable-bliss-326713",
  "private_key_id": "504d9d16d069bf996605aa8512cef88d0c233ef7",
  "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEugIBADANBgkqhkiG9w0BAQEFAASCBKQwggSgAgEAAoIBAQCw2KSoCovRDw0P\ntic7Zi6G+fh/ukgcLcCcpRGnZqfxMarrA9ftSoVG0Ium/wgFcyq02m6MJ5uqCQKZ\n6SAkd56fnE4Mj8TPZ9wdrpmSBc38bW6MaQ4t1rZCE0tXtnc2qmQ3u7Pjw7bWFX4X\npqwJ7qr5vIjxCt91IhVFjmdACRRxHXsTALkEWbWGunei606dIQN0LEHNJQTeOm7c\nZUP/0vOA0IIg4XMOn9Mgwt0VCbLuUh/EtoF9U+nc15Y6qDVzK3FZqz+d2IgGlVqg\nMeOI5Y9chmR7+9ezsbwghK6+FkNlDQN2s4m9rlCIPzGs1B2hoT7RM3dGsMFohF3W\nb2cOtFA1AgMBAAECgf9/BIsk3q/pi8un37bROzc2ZdDhnd0ztWc5dzvNzAFxXCdt\naiDG/5fbCeF4wacvLzNpnrTorWK+aJl8QRxYQue/K7lmlIccpXWuuy9qsgjRZktP\nON1LmP+QxJmuHgRG6dmhxRvE3ilaA6zVXQ7esPAPgx+qnsiyzu9+5R5kfXZfKZUW\nqO0fJlRVy+n2H9gPa80wLBmLC0WggX70aDLSVOzPSM25f9kLALdMQOq16KiMcgFt\nZXM1J8h38lzk5O/5DP3+YK6yIw6Sk0ComiA8cM0WLjh2dSxOzsiu0euaZfzCg3mI\n0PihI1Zn3y+0u4aLhQzWrRLjkm5YL/mczDE/dV0CgYEA3OxA6FW5jSmFqNOXiPkn\nOrKI899aHwdefXv4IyJBx56p4m/OtYhLCcfqZVD0va2STTrJxTkxd1IoOE3iazmk\n+59OaA6nHZ+DJuPaHE38yx3VlP2Xu9x98FChfh737y8MkCGDG0SlZPLOWYtAqm34\n0ajs9dGddBa6065UuJ7vHDsCgYEAzOzUNvGQLcsp0pwUv346vDwEHOGrNEznqN9m\ntA6O2MHD/Uhow5+1C9Q6qdSjomaEgQAQyLSZ8jDQMfNgPuWKiuhBxNJqoP+pmLmV\nDX9gjrYhaC069GXiM9XLN4eBgC+8OUpJ0uGf5y9xIGc7Mzwggu9SzRpbU4ExFkUE\nEd4ILk8CgYACOmvJLBwr/VA4zLGJkyG/KM/Szdzl1Tgxwc9V42GQdtw1yXTnRdGj\n7p1BmlhtlVOVbdNdJt+F2ZFISBjRhgpQ82FiEUvEM7ip6sBnaq3zCoD9+LTQE0A9\n7UJDuGI9ylaxGXVuD/ovE3fbVVk4ntVUoASL0ippYlVOD0MRfaDhwwKBgH3iYj7a\nR8qJykhRj0FPRr+JUrF3Q5eeQpTvn4SuiLvI1B/MkmbRvgLlPaBEOr0xouYn5yeU\nIhmwOMTmj9IzadcI9BX3eH8sbr+eCV1ASU2BvqXjWxr1V26mP+79s+TN7h93bkA4\nT+VRtHnGUIyZvBMS91d9gGqcq03V2bksHvRrAoGAXBLfNu19CMByPnIOTpA2J4VP\nGTr1EXjx0QUYXmuR0IQmEfavdKuWfPzIRxKiA/bLKc2NKi3XemAeZ/3gsCP1YCSV\npZOCpVlfsCO/egjwgU0LYzaZG9HelNIyogL06fXBvyxAo950xW8YnAdQ3F30BhP0\nK/HgLhN6rd7v4hdZb7o=\n-----END PRIVATE KEY-----\n",
  "client_email": "speech2text-646@capable-bliss-326713.iam.gserviceaccount.com",
  "client_id": "102868561834162307208",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/speech2text-646%40capable-bliss-326713.iam.gserviceaccount.com"
})

export default {
  name: "HomeView",
  data: () => ({
  }),
  async mounted() {

    const client = new speech.SpeechClient({
      auth:authClient
    });


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
  },
}
</script>

<style scoped>

</style>
