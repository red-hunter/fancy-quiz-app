<template>
  <div>
    <div>
      <b-jumbotron>
        <template v-slot:lead>{{currentQuestion.question}}</template>

        <hr class="my-4" />

        <p>
          <b-list-group v-for="(answer, ansIndex) in answers" :key="ansIndex">
            <b-list-group-item
              :class="answerClass(ansIndex)"
              @click="selectedAnswer(ansIndex)"
            >{{answer}}</b-list-group-item>
          </b-list-group>
        </p>

        <b-button
          variant="primary"
          href="#"
          @click="submitAnswer"
          :disabled="selectedIndex === null || answered"
        >Submit</b-button>
        <b-button @click="next" variant="success" href="#">Next</b-button>
      </b-jumbotron>
    </div>
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
  computed: {
    answers: function() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  watch: {
    currentQuestion() {
      this.selectedIndex = null;
      this.shuffleAnswers();
      this.answered = false;
    }
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    };
  },
  methods: {
    selectedAnswer: function(ansIndex) {
      this.selectedIndex = ansIndex;
      // console.log(this.index)
    },
    shuffleAnswers: function() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer: function() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.increment(isCorrect);
      this.answered = true;
    },
    answerClass: function(ansIndex) {
      let answerClass = "";
      if (!this.answered && this.selectedIndex === ansIndex) {
        answerClass = "selected";
      } else if (this.answered && this.correctIndex === ansIndex) {
        answerClass = "correct";
      } else if (
        this.answered &&
        this.selectedIndex === ansIndex &&
        this.correctIndex != ansIndex
      ) {
        answerClass = "incorrect";
      }
      return answerClass;
    }
  },
  mounted() {
    this.shuffleAnswers();
  }
};
</script>

<style scoped>
.btn {
  margin: 0px 10px;
}
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}
.selected {
  background-color: lightblue;
}
.selected:hover {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: red;
}
</style>