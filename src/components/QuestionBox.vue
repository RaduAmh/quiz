<template>
  <div class="question-box-container">
    <b-jumbotron v-if="!showScore">
      <template v-slot:lead>
        <span v-if="currentQuestion" v-html="currentQuestion.question"></span>
        <span v-else>Loading...</span>
      </template>

      <hr class="my-4" />
      <b-list-group v-if="currentQuestion">
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click="selectAnswer(answer)"
          :class="[selectedAnswer === answer ? 'selected': '',
          submited ? (selectedAnswer === answer ? (isCorrect ? 'correct' : 'incorrect') : '') : '',
          submited ? (currentQuestion.correct_answer === answer && !isCorrect ? 'correct' : '') : '']"
        >
          <span v-html="answer"></span>
        </b-list-group-item>
      </b-list-group>

      <b-button pill variant="primary" @click="submitAnswer" :disabled="submited">Submit</b-button>
      <b-button pill variant="success" @click="$emit('next')" :disabled="!submited">Next</b-button>
    </b-jumbotron>
    <b-jumbotron v-else>
      <template v-slot:lead>
        <h1>You've scored {{score}} points!</h1>
      </template>
      <hr class="my-4" />
      <h4>Wanna try another one?</h4>
      <b-button pill variant="primary" @click="$emit('restart')">Restart</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  name: "QuestionBox",
  props: {
    currentQuestion: Object,
    showScore: Boolean,
    score: Number
  },
  data() {
    return {
      selectedAnswer: "",
      submited: false,
      isCorrect: false
    };
  },
  computed: {
    answers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      for (let i = answers.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [answers[i], answers[j]] = [answers[j], answers[i]];
      }
      return answers;
    }
  },
  watch: {
    currentQuestion() {
      this.selectedAnswer = "";
      this.submited = false;
      this.isCorrect = false;
    }
  },
  methods: {
    selectAnswer(answer) {
      this.selectedAnswer = answer;
    },
    submitAnswer() {
      this.submited = true;
      this.isCorrect =
        this.selectedAnswer === this.currentQuestion.correct_answer
          ? true
          : false;
      this.$emit("submit", this.isCorrect);
    }
  }
};
</script>

<style scoped>
.jumbotron {
  padding: 32px;
  margin: 16px 0;
}
.list-group {
  margin-bottom: 10px;
}
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}
.btn {
  margin: 5px 5px;
  width: 100px;
}
.selected {
  background-color: #e9ecef;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: #ffcccb;
}
</style>
