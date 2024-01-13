<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <h3>Fancy quiz app by <a href="https://madhusudan.live">Madhusudan</a> </h3>
      <v-spacer></v-spacer>
      <h4>score: {{ numCorrect }} / {{ numTotal }}</h4>
    </v-app-bar>

    <v-main>
      <v-container class="grey lighten-5">
        <v-row no-gutters>
          <v-col cols="12" md="12" lg="9" 
          class="mx-auto" v-if="questions.length">
          <QuestionBox style="margin: 0 auto; margin-top: 2em"
            :next="next"
            :currentQuestion="questions[index]"
            :increment="increment"
          />
          </v-col>
          <v-col cols="3"  offssm="3" md="3"  v-else-if="!err" >
            <v-spacer></v-spacer>
            <v-progress-circular class="mx-auto" :size="70" :width="7" color="purple" indeterminate></v-progress-circular>
            <v-spacer></v-spacer>
          </v-col>

          <div v-else>{{ err }}</div>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>


<script>
import QuestionBox from "./components/QuestionBox";
export default {
  name: "App",
  components: {
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
      .catch(e => {
        this.err = e.message;
        console.log(e.message);
      });
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
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    }
  }
};
</script>
