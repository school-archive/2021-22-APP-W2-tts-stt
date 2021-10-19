<template>
  <div>
    <v-row>
      <v-col sm="3">
        <v-card>
          <v-card-title>Speech to text</v-card-title>
          <v-card-text>
            <v-text-field
              label="Result"
              disabled
              :value="sttResult"
              />
          </v-card-text>
          <v-card-actions>
            <v-btn
                text
                color="teal accent-4"
                @click="isRecognising ? recognition.stop() : startRecognition()"
            >
              {{ isRecognising ? 'Stop' : 'Start' }} recognition
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>

      <v-col sm="3">
        <v-card>
          <v-card-title>Text to speech</v-card-title>

          <v-text-field
              label="Text"
              hide-details="auto"
          ></v-text-field>
          <v-card-actions>
            <v-btn
                class="ml-auto"
                icon
                color="accent-4"
            >
              <v-icon>mdi-play</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  name: "WebkitView",
  data: () => ({
    recognition: null,
    speechRecognitionList: null,
    lang: 'de-DE',
    sttResult: null,
    isRecognising: false,
  }),
  mounted() {
    let SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition
    let SpeechGrammarList = window.SpeechGrammarList || window.webkitSpeechGrammarList

    this.recognition = new SpeechRecognition();
    this.speechRecognitionList = new SpeechGrammarList();
    let grammar = '#JSGF V1.0;'


    this.speechRecognitionList.addFromString(grammar, 1);


    this.recognition.grammars = this.speechRecognitionList;
    this.recognition.continuous = true;
    this.recognition.lang = this.lang;
    this.recognition.interimResults = true;

    this.recognition.onresult = (event) => {
      console.log("onresult", event.results)
      console.log("result: ", event.results[0][0].transcript)
      console.log('Confidence: ' + event.results[0][0].confidence);
      this.sttResult = event.results[0][0].transcript;
    }


    this.recognition.onspeechend = (e) => {
      console.log(this.recognition, e)
      this.recognition.stop();
      this.isRecognising = false;
    }

    this.recognition.onmatch = e => {
      console.log("match", e)
    }

    this.recognition.onnomatch = (event) => {
      console.log('I didnt recognize that color.', event);
    }

    this.recognition.onerror = (event) => {
      console.log('Error occurred in recognition: ' + event.error);
    }
  },
  methods: {
    startRecognition() {
      this.recognition.start();
      this.isRecognising = true;
      console.log('Ready to receive a command.');
    }
  }
}
</script>

<style scoped>

</style>
