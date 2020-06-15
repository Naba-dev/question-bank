<template>
<div>
  <b-jumbotron>

    <template v-slot:lead>
        <h3> {{ currentQuestion.question }} </h3>
    </template>

    <hr class="my-4">
    <b-list-group>
      <b-list-group-item v-for="(ans,index) in allAnswers" :key="index" @click="selectedAnswer(index)">
        {{ ans }}
      </b-list-group-item>
    </b-list-group>
<!-- 
    <p v-for="(ans,index) in allAnswers" :key="index">
      {{ ans }}
    </p> -->
<!-- Initially the "selectIndexValue" is null, so the disabled attribute becames true and the button gets disabled -->
    <b-button variant="primary" href="#" class="mr-4" 
    @click="submitAns" 
    :disabled="selectedIndexValue === null || answered === true"> 
        Submit 
    </b-button>
    
    <b-button variant="success" href="#" @click="nextQuestion"> Next </b-button>
  </b-jumbotron>
</div>
</template>

<script>
    export default {
        props: {
            currentQuestion: Object,
            nextQuestion: Function,
        },
        data() {
          return {
            selectedIndexValue: null,
            answered: false
          }
        },
        watch: {
          currentQuestion() {
            this.answered = false  
          }
        },
        computed: {
          // since the corrent and incorrect answers are provided in seperate properties in the API responce, so here we are joining both of them and storing in single array "answers" and looping through it in the template using v-for
          allAnswers() {
            var answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
          }
        },
        methods:{
          selectedAnswer(index){
              this.selectedIndexValue = this.allAnswers[index]
              // console.log(this.selectedIndexValue)
          },
          submitAns() {
            let isCorrect = false
            this.answered = true

            if(this.currentQuestion.correct_answer === this.selectedIndexValue)
            {
             isCorrect = true
            }     
            this.$emit('incrementCorrectAnsCount', isCorrect)           
          },

        }
      }
</script>

<style scoped>
  .list-group {
    margin-bottom: 20px;
  }
  .list-group-item:hover {
    background: #EEE;
    cursor: pointer;
  }

  .selected {
    background-color: lightblue;
  }

  .incorrect {
    background-color: darkred;
  }

  .correct {
    background-color: green;
  }
</style>