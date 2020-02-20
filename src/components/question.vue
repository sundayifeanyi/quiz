<template>
  <div class="questionBoxcontianer">
    <b-jumbotron>
      <template slot="lead">{{currentQuestion.question}}</template>
      <hr />
      <b-list-group>
        <b-list-group-item
          class="list-group"
          v-for="(answer,index) in answers"
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="anwsweredClass(index)"
        > {{ answers }} </b-list-group-item>
      </b-list-group>
      <b-button
        @click="submit()"
        variant="primary"
        :disabled="selectedIndex === null || answered"
      >submit</b-button>
      <b-button @click="next()" variant="success">next</b-button>
    </b-jumbotron>
  </div>
</template>
<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      currentIndex: null,
      shuffledAnswers: [],
      answered: false
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffledAnswers();
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    submit() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    }
  },
  shuffledAnswers() {
    let answers = [
      ...this.currentQuestion.incorrect_answers,
      this.currentQuestion.correct_answer
    ]
    this.shuffledAnswers = _.shuffle(answers);
    this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
  },
  anwsweredClass(index) {
    let answeredclass = "";
    if (!this.answered && this.selectedIndex === index) {
      answeredclass = "selected";
    } else if (this.answered && this.correctIndex === index) {
      answeredclass = "correct";
    } else if (
      this.answered &&
      this.selectedIndex === index &&
      this.correctIndex !== index
    ) {
      answeredclass = "correct";
    }
    return answeredclass;
  },

  mounted() {
    this.shuffledAnswers();
  }
};
</script>
<style scoped>
.list-group {
  margin-bottom: 10px;
}
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}
.btn {
  margin: 0 5px;
}
.selected {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: lightcoral;
}
</style>