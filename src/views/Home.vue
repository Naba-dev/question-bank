<template>
  <div class="home">
    <Header 
      :correctAnswers = "numCorrectAns"
      :totalAnswers = "numTotalAns">
    </Header>
    <b-container class="bv-example-row col-md-5">
      <b-row>
        <b-col>
           <!-- v-if="questions.length > 0" is added so that the QuestionBox component loads only after the api data has been fetched and is available for use by the QuestionBox component -->
           <!-- :(colon) is the shorthand way of writing v-bind -->
            <QuestionBox v-if="questions.length > 0" v-bind:currentQuestion = "questions[index]" :nextQuestion = "next" @incrementCorrectAnsCount = "updateCorrectAnsCount($event)"/> 
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue'
import QuestionBox from '@/components/QuestionBox.vue'

export default {
  name: 'Home',
  components: {
    Header,
    QuestionBox
  },
  data: function() {
    return {
      questions: [],
      index: 0,
      numCorrectAns: 0,
      numTotalAns: 0
    }
  },
  methods: {
    next() {
      this.index++;
    },
    updateCorrectAnsCount(isCorrectValue) {
      if(isCorrectValue) {
          this.numCorrectAns++
      }
      this.numTotalAns++
      // console.log(isCorrectValue)
}
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=9&type=multiple')
        .then((response) => response.json())
        .then((jsonData) => {
          this.questions = jsonData.results 
        })
  }
}
</script>
