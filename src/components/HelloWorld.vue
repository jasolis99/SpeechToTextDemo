<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Result</h2>
    <h3 v-if="result">{{ result }}</h3>
    <h3 v-else>Click the button to start</h3>
    <h4 v-if="log" style="color: red">{{ log }}</h4>
    <button @click="startSpeak()">Start</button>
  </div>
</template>

<script>
const SpeechRecognition =
  window.SpeechRecognition || window.webkitSpeechRecognition;

const recognition = new SpeechRecognition();

recognition.maxAlternatives = 1;

export default {
  name: "HelloWorld",
  data() {
    return {
      msg: "Welcome to the Speech Recognition demo",
      result: null,
      log: null,
    };
  },
  methods: {
    startSpeak() {
      recognition.lang = "es-ES";
      recognition.start();
      this.recog();
    },
    recog() {
      recognition.addEventListener("speechstart", () => {
        this.log = "Start to Speak to the mic";
      });

      recognition.addEventListener("speechend", () => {
        recognition.stop();
      });

      recognition.addEventListener("result", (e) => {
        console.log(e);

        this.log = "Result has been detected";
        let last = e.results.length - 1;
        let text = e.results[last][0].transcript;

        this.result = text;
      });

      recognition.addEventListener("error", (e) => {
        this.results = "Error: " + e.error;
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
