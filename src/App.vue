<template>
  <div id="app">
     <headers :numCorrect="numCorrect" :total="total" />
    <b-container>
      <b-col>
        <b-col sm="6" offset="3">
          <questions v-if="questions.length" :currentQuestion="questions[index]" :next="next" :increment ="increment" />
        </b-col>
      </b-col>
    </b-container>
  </div>
</template>

<script>
import headers from "./components/header";
import questions from "./components/question";

export default {
  name: "app",
  components: {
    headers,
    questions
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      total: 0
    };
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrect++
      }
      this.total++
      }
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&category=9&type=multiple", {
      method: "get"
    })
      .then(response => {
        return response.json();
      })
      .then(jsonData => {
        this.questions = jsonData.results;
      });
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
