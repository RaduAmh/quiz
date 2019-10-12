<template>
  <div id="app">
    <Header :questionIndex="index + 1" :score="score" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm></b-col>
        <b-col sm="auto">
          <QuestionBox
            :currentQuestion="questions[index]"
            :showScore="showScoreInBox"
            :score="score"
            @next="incrementIndex"
            @submit="changeScore"
            @restart="resetAll"
          />
        </b-col>
        <b-col sm></b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header";
import QuestionBox from "./components/QuestionBox";

export default {
  name: "app",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      score: 0,
      showScoreInBox: false
    };
  },
  methods: {
    incrementIndex() {
      if (this.index < 9) {
        this.index++;
      } else {
        this.showScoreInBox = true;
      }
    },
    changeScore(isCorrect) {
      if (isCorrect === true) {
        this.score += 10;
      }
    },
    resetAll() {
      this.getQuestions();
      this.index = 0;
      this.score = 0;
      this.showScoreInBox = false;
    },
    getQuestions() {
      fetch("https://opentdb.com/api.php?amount=10&type=multiple", {
        method: "get"
      })
        .then(response => response.json())
        .then(jsonData => (this.questions = jsonData.results));
    }
  },
  mounted: function() {
    this.getQuestions();
  }
};
</script>

<style>
@import "./assets/global.css";
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
