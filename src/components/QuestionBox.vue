<template>

<div class="question-box-container">
  <b-jumbotron>
    
      <template slot="lead">
      {{currentQuestion.question}}
      </template>

      <hr class="my-4">

        <b-list-group>
          <b-list-group-item
          v-for=" (answer, index) in answers" 
          :key="index"
          @click="selectAnswer(index)"
          :class="[selectedIndex === index ? 'selected' : '']"
          >
             {{ answer }}
          </b-list-group-item>
          
        
        </b-list-group>

    
      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
       
      >
        Submit
      </b-button>
      <b-button @click="next" variant="success" href="#">
          NEXT
      </b-button>
  </b-jumbotron>
</div>
        
    
</template>

<script>

import _ from 'lodash'

export default {
  props:  {
    currentQuestion: Object,
    next: Function,
    increment: Function

  },
  data(){
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },
  computed: {
    answers(){
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler(){
        this.selectedIndex = null
        this.answered = false
        this.shuffleAnswers()
      }
    }
  },
  methods: {
     selectAnswer(index){
       this.selectedIndex = index
       
     },
     submitAnswer() {
      let isCorrect = false
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)
      
    },
     shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      

     }

  },
}
</script>
<style scoped>
.list-group{
  margin-bottom: 15px;
}

.btn {
  margin: 15px 15px 0 15px;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorect {
  background-color: red;
}
.list-group-item:hover {
  background: #EEE;
  cursor: pointer;
}
</style>