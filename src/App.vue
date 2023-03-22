<template>
    <Header />
    <Content 
      v-if="questions.length" 
      :currentQuestion="questions[index]"
      :next="next"
      :increment="increment"
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />
</template>

<script >
import Header from "./components/Header.vue";
import Content from "./components/Content.vue";

export default {
  name: 'App',
  components: {
    Header,
    Content
  },    
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
    }
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++;
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&difficulty=medium&type=multiple')
      .then(response => response.json())
      .then(jsonData => {
        this.questions = jsonData.results
      })
  }
}
</script>

<style scoped>
  
</style>
