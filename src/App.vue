<template>
  <div id="app">
    <Heading :totalAnswered="totalAnswered" :currentScore="currentScore"></Heading>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuizBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Heading from "./components/Heading";
import QuizBox from "./components/QuizBox";

export default {
  name: "App",
  components: {
    Heading,
    QuizBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      currentScore: 0,
      totalAnswered: 0
    };
  },
  methods: {
    next: function() {
      this.index++;
    },
    increment: function(isCorrect) {
      if (isCorrect) {
        this.currentScore++;
      }
      this.totalAnswered++;
    }
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=20", { method: "get" })
      .then(Response => {
        return Response.json();
      })
      .then(jsonData => {
        this.questions = jsonData.results;
      });
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
}
</style>
