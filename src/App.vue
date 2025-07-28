<template>
  <div>
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
      <button class="send" @click="submitAnswer">Send</button>
    </template>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      submittedAnswer: false,
    };
  },
  methods: {
    submitAnswer() {
      if (!this.chosenAnswer) {
        alert("Select an answer");
      } else {
        this.submittedAnswer = true;
        if (this.chosenAnswer == this.correctAnswer) {
          alert("You are Correct");
        } else {
          alert("You are Incorrect");
        }
      }
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
    this.axios
      .get("https://opentdb.com/api.php?amount=10&category=18")
      .then((response) => {
        this.question = response.data.results[0].question;
        this.incorrectAnswers = response.data.results[0].incorrect_answers;
        this.correctAnswer = response.data.results[0].correct_answer;
      });
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
