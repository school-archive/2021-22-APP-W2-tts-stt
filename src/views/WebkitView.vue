<template>
  <div>
    <v-row align="center" class="card">
      <v-col sm="1"/>
      <v-col sm="4">
        <v-card class="card">
          <v-card-title><v-icon class="mr-3">mdi-microphone</v-icon>Speech to Text</v-card-title>
          <v-text-field
              label="Language"
              filled
              :value="sttLang"
          />
          <v-text-field
              label="Result"
              disabled
              filled
              :value="sttResult"
          />
          <v-card-actions>
            <v-btn
                text
                color="teal accent-4"
                @click="isRecognising ? recognition.stop() : startRecognition()"
            >
              <v-icon class="mr-1">{{isRecognising ? 'mdi-radiobox-marked' : 'mdi-record'}}</v-icon>
              {{ isRecognising ? 'Stop' : 'Start' }} recognition
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>

      <v-col sm="2" class="d-flex flex-row justify-center">
        <v-btn @click="translateText">
          Translate
          <v-icon>mdi-arrow-right</v-icon>
        </v-btn>
      </v-col>

      <v-col sm="4">
        <v-card class="card">
          <v-card-title><v-icon class="mr-3">mdi-message-processing</v-icon>Text to Speech</v-card-title>
          <v-text-field
              label="Language"
                        filled
                        v-model="selectedTtsLang"/>
          <v-text-field
              label="Text"
              hide-details="auto"
              filled
              v-model="ttsText"
          ></v-text-field>
          <v-card-actions>
            <v-btn
                class="ml-auto"
                icon
                color="accent-4"
                @click="speakText()"
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
import axios from "axios";

export default {
  name: "WebkitView",
  data: () => ({
    recognition: null,
    speechRecognitionList: null,
    lang: 'de',
    sttResult: '',
    sttLang: 'de',
    isRecognising: false,
    selectedTtsLang: 'en',
    ttsText: "",
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
    },

    speakText(text=this.ttsText, lang=this.selectedTtsLang) {
      let utterance = new window.SpeechSynthesisUtterance(text);
      utterance.lang = lang;
      window.speechSynthesis.speak(utterance);
      console.log("started to speak")
    },
    translateText(text=this.sttResult, srcLang=this.sttLang, targetLang=this.selectedTtsLang) {
      console.log(srcLang, targetLang, text);

      const options = {
        method: 'POST',
        url: 'https://deep-translate1.p.rapidapi.com/language/translate/v2',
        headers: {
          'content-type': 'application/json',
          'x-rapidapi-host': 'deep-translate1.p.rapidapi.com',
          'x-rapidapi-key': 'ae4205d3c9mshc1c3840266dedfcp140177jsn23d76fa8bd27'
        },
        data: {q: this.sttResult, source: srcLang, target: targetLang}
      };

      axios.request(options).then(response => {
        console.log(response.data);
        this.ttsText = response.data.data.translations.translatedText;
      }).catch(function (error) {
        console.error(error);
      });

    }
  }
}
</script>

<style scoped>
.card {
  height: 300px;
}
</style>
