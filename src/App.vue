<template>
  <div id="app">
    <Header 
    :numCorrect="numCorrect"
    :numTotal="numTotal" />

    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox v-if="questions.length" 
          :next="next" 
          :currentQuestion="questions[index]" 
          :increment="increment"/>
          <div v-else-if="!err" class="loading">
            loading...
          </div>
          <div v-else>
            {{ err }}
          </div>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header";
import QuestionBox from "./components/QuestionBox";
export default {
  name: "App",
  components: {
    Header,
    QuestionBox
  },

  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&category=27&ype=multiple", {
      method: "get"
    })
      .then(resp => {
        return resp.json();
      })
      .then(jsonData => {
        this.questions = jsonData.results;
      })
      .catch((e) =>{
        this.err = e.message;
        console.log(e.message);
        
      })
  },
  data() {
    return {
      questions: [],
      index: 0,
      err: null,
      numCorrect: 0,
      numTotal: 0
    };
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect){
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    }
  }
};
</script>

<style>
*,
*::after,
*::before {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
}

.list-group{
  margin-bottom: 1rem;
}

.btn{
  margin: 0 1rem;
}
</style>
