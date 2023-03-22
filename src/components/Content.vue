<template>
  <main><br><br>
    <div class="container">
      <small>Select the correct answer: </small><br><br>
      <div class="jumbotron">
        <h1>Your result: 
          <span class="badge badge-success">{{ numCorrect }}</span> | 
          <span class="badge badge-danger">{{ numTotal }}</span>
        </h1>
        <hr>
        <p><b>Question: </b>{{ currentQuestion.question }}</p>
        <hr>
        <div>
          <b class="">List of Answers: </b>
          <ul class="list-group" v-for="(answer, index) in answers" :key="index">
            <li class="list-group-item" 
              @click="selectAnswer(index)"
              :class="cssClass(index)"
            >{{ answer }}</li>
          </ul>
        </div>
        <hr>
        <div class="pull-right">
          <button @click="submitAnswer" :disabled="selectedIndex === null || answered" type="button" class="btn btn-primary">Submit</button>
          <button @click="next" type="button" class="btn btn-warning">Next Queston</button>
        </div>
      </div>
      <div class="table-responsive">
        <table class="table">
          <thead>
            <tr>
              <th scope="col">#</th>
              <th scope="col">First</th>
              <th scope="col">Last</th>
              <th scope="col">Handle</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <th scope="row">1</th>
              <td>Love</td>
              <td>Otto</td>
              <td>@mdo</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </main>
</template>

<script>
import _ from "lodash"

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
    numCorrect: Number,
    numTotal: Number,
  },
  data(){
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
      correctIndex: null,
    }
  },
  watch: {
    currentQuestion: {
      immediate: true, // This shuffle question direct when the page load 
      handler() {
        this.selectedIndex = null,
        this.answered = false,
        this.shuffleAnswers(); 
      }
    }
  },
  methods: {
    cssClass(index) {
      let answerClass = '';
      if (!this.answered && this.selectedIndex === index) {
        answerClass = "selected"
      } else if(this.answered && this.correctIndex === index){
        answerClass = "correct"
      } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
        answerClass = "incorrect"
      }
      return answerClass
    },
    selectAnswer(index) {
      this.selectedIndex = index,
      console.log(this.currentQuestion.correctIndex)
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  }
}
</script>

<style>
  button{
    margin: 0 10px 0 0;
  }
  .list-group{
    margin: 10px 0;
  }
  .list-group-item:hover{
    cursor: pointer;
    border: 1px solid rgb(98, 98, 98);
  }
  .selected{
    border: 1px solid rgb(98, 98, 98);
  }
  .correct{
    border: 1px solid green;
  }
  .incorrect{
    border: 1px solid maroon;
  }
</style>
