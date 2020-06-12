<template>
  <div class="question-box" style="text-align: center">
    <b-jumbotron header="Quiz" lead>
      <template>{{ currentQuestion.question }}</template>

      <hr class="my-4" />

      <v-container>
        <v-btn text
          class="list mx-5"
          v-for="(option, index) in shuffledAnswers"
          :key="index"
          @click.prevent="select(index)"
          :class="ansClass(index)"
        >{{ option }}</v-btn>
      </v-container>

      <hr />

      <v-btn class="mx-5" raised color="primary"
      @click="submitAnswer"
      :disabled="selected === null || answered"
      >Submit</v-btn>
      <v-btn raised color="success" 
      @click="next" :disabled="!answered">Next</v-btn>
    </b-jumbotron>
  </div>
</template>

<script>

import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment:Function
  },
  data() {
    return {
      selected: null,
      shuffledAnswers: [],
      answered: false,
      correctIndex: null
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  methods: {
    select(index) {
      console.log(index);
      this.selected = index;
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer(){
      let isCorrect = false

      if(this.selected === this.correctIndex){
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)
    },
    ansClass(index){
      let style = '';
      if (!this.answered && this.selected === index) {
        style = 'selected'; 
      } else if (this.answered && this.correctIndex === index) {
        style = 'correct'
      } else if (this.answered && this.selected === index && this.correctIndex !== index) {
        style = 'wrong'
      } else{
        style = ''
      } 
      return style;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler(){
        this.selected = null;
        this.shuffleAnswers()
        this.answered = false
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.list {
  transition: all 0.3s ease-in-out;
  cursor: pointer;

  &:hover {
    background-color: rgba($color: #000000, $alpha: 0.3);
    color: aliceblue;
  }
}

.selected {
  background-color: rgba($color: blue, $alpha: 0.6);
  color: aliceblue !important;
}
.correct {
  background-color: green;
  color: aliceblue !important;
}

.wrong {
  background-color: orangered;
  color: aliceblue !important;
}
</style>
