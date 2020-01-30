<template>
  <div id="app">
    <Header 
      v-bind:numCorrect="numCorrect"
      v-bind:numTotal="numTotal"
    />

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6">
          <QuestioBox 
            v-if="questions.length"
            v-bind:currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>

  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestioBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestioBox
  },
  data () {
    return {
      questions: [],
      index: 0,
      numTotal: 0,
      numCorrect: 0
    }
  },
  methods: {
    next () {
      this.index++;
    },
    increment (isCorrect) {
      this.numTotal++;

      if(isCorrect){
        this.numCorrect++;
      }
    }
  },
  mounted: function () {
    fetch ( 'https://opentdb.com/api.php?amount=10&category=18&difficulty=medium&type=multiple', {
      method: 'get'
    } )
      .then ( (response) => {
        return response.json();
      })
        .then ( (quizQuestions) => {

          if(quizQuestions.results){
            this.questions = quizQuestions.results;
          }
        })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
