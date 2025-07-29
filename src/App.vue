<template>
  <div>
    <ScoreBoard :winCount="winCount" :loseCount="loseCount" />
    <template v-if="question">
      <h1 v-html="question"></h1>
      <template v-for="(answer, index) in this.answers" :key="index">
        <input
          :disabled="this.submittedAnswer"
          type="radio"
          name="options"
          :value="answer"
          v-model="chosenAnswer"
        />
        <label v-html="answer"></label><br />
      </template>
      <button v-if="!submittedAnswer" class="send" @click="submitAnswer">
        Send
      </button>
      <section v-if="submittedAnswer" class="result">
        <h4
          v-if="this.chosenAnswer == this.correctAnswer"
          v-html="
            '&#9989; Congratulations, your selection ' +
            chosenAnswer +
            ' is correct.'
          "
        ></h4>
        <h4
          v-else
          v-html="
            '&#10060; Sorry, your selection ' +
            chosenAnswer +
            ' is incorrect, the correct answer is ' +
            correctAnswer +
            ' .'
          "
        ></h4>
        <button class="send" type="button" @click="getNewQuestion()">
          Next Question
        </button>
      </section>
    </template>
  </div>
</template>

<script>
import ScoreBoard from "./components/ScoreBoard.vue";

export default {
  name: "App",
  components: {
    ScoreBoard,
  },

  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      submittedAnswer: false,
      winCount: 0,
      loseCount: 0,
    };
  },
  methods: {
    submitAnswer() {
      if (!this.chosenAnswer) {
        alert("Select an answer");
      } else {
        this.submittedAnswer = true;
        if (this.chosenAnswer == this.correctAnswer) {
          this.winCount++;
          console.log("You are Correct");
        } else {
          this.loseCount++;
          console.log("You are Incorrect");
        }
      }
    },
    getNewQuestion() {
      this.submittedAnswer = false;
      this.chosenAnswer = undefined;
      this.question = undefined;
      this.axios
        .get(
          "https://opentdb.com/api.php?amount=30&category=18&difficulty=hard"
        )
        .then((response) => {
          this.question = response.data.results[0].question;
          this.incorrectAnswers = response.data.results[0].incorrect_answers;
          this.correctAnswer = response.data.results[0].correct_answer;
        });
    },
  },
  computed: {
    answers() {
      var answers = [...this.incorrectAnswers];
      var rN = Math.round(Math.random() * answers.length);
      answers.splice(rN, 0, this.correctAnswer);
      return answers;
    },
  },
  created() {
    this.getNewQuestion();
  },
};

//
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type="radio"] {
    margin: 12px 4px;
  }
  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1x solid #1867c0;
    border-radius: 10px;
    cursor: pointer;
  }
}
</style>
