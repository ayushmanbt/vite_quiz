<template>
  <div>
    <div class="topbar">
      <h1>Vite Quizs</h1>
    </div>

    <div class="question_status_cover">
      <div class="question_status">
        <div class="circle" v-for="(status,index) in answeringStatus" :key="index" :class="determineClass(status)">
          <p v-if="status == 1">&checkmark;</p>
          <p v-if="status == 2">&cross;</p>
        </div>
      </div>
    </div>
    
    <div class="container">
      <div v-if="!questionsOver">
        <QuizQuestion :question="questions[currentIndex]" @answerEvent="answer"/>
      </div>

      <div class="game_end" v-if="questionsOver">
        <h3 class="wait_text">All Questions over</h3>
        <p> <strong>Correct Answers:</strong> {{correctAnswers}}</p>
        <p> <strong>Wrong Answers:</strong> {{wrongAnswers}}</p>
        <a class="btn" href="/">Replay</a>  
      </div>
      
      <h3 class="wait_text" v-if="waiting">Wait...</h3>
    </div>
  </div>
</template>

<script>
import QuizQuestion from './components/QuizQuestion.vue';
import questionObjects from './assets/questions.json';

const answeringStatus = {
  currentlyOn: 0,
  correctAnswer: 1,
  wrongAnswer: 2,
  notArrived: 4,
}

export default {
  name: 'App',
  components: {
    QuizQuestion
  },
  data(){
    return{
      currentIndex: 0,
      questions: questionObjects,
      correctAnswers: 0,
      wrongAnswers: 0,
      waiting: false,
      answeringStatus: [],
      questionsOver: false
    }
  },

  beforeMount(){
    for(let i = 0; i<this.questions.length; i++){
      this.answeringStatus[i] = answeringStatus.notArrived
    }

    this.answeringStatus[0] = answeringStatus.currentlyOn
  },

  watch: {
    currentIndex: function(oldVal, newVal) {
      this.answeringStatus[newVal + 1] = answeringStatus.currentlyOn
    }
  },

  methods:{
    answer(index){
      if(this.waiting) return;
      this.waiting = true;
      if(index === this.questions[this.currentIndex].correctAnswerIndex){
        this.correctAnswers++;
        this.answeringStatus[this.currentIndex] = answeringStatus.correctAnswer;
      } 
      else{
        this.wrongAnswers++;
        this.answeringStatus[this.currentIndex] = answeringStatus.wrongAnswer;
      } 
      setTimeout(() => {if(this.questions.length - 1 !== this.currentIndex) this.currentIndex++; else this.questionsOver = true; this.waiting = false},1000)
    },
    determineClass(status){
      if(status === answeringStatus.currentlyOn) return "currently-on"
      if(status === answeringStatus.correctAnswer) return "correct-answer"
      else if(status === answeringStatus.wrongAnswer) return "wrong-answer"
      else return "not-arrived"
    }
  }
}
</script>

<style scoped>

.topbar{
  background-color: #434343;
  color: white;
  text-align: center;
  padding: 20px;
  
}

.topbar h1{
  font-family: 'Montserrat Alternates', sans-serif;
  font-weight: bold;
  font-size: 2.5rem;
}

.question_status_cover{
  background-color: #E5E5E5;
  padding: 15px 0;
}

.question_status{
  display: flex;
  margin: 0 auto;
  justify-content: space-evenly;
  align-content: center;
  font-weight: bold;
  max-width: 400px;
}

.circle{
  width: 40px;
  height: 40px;
  border-radius: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.3rem;
  font-weight: bold;
}

.currently-on{
  background-color: #ff9900;
}

.correct-answer{
background-color: #299602;
}

.wrong-answer{
  background-color: #FF3535;
}

.not-arrived{
  background-color: #464646;
}

.container{
  width: 80%;
  max-width: 800px;
  margin: 20px auto;
}

.game_end{
  text-align: center;
  font-size: 1rem;
}

.game_end p{
  margin: 10px 0
}

.btn{
  width: 100px;
  margin: 0 auto;
  border-radius: 15px;
  font-weight: bold;
  transition: background 0.2s ease-in-out;
}

.btn:hover{
  background: chartreuse;
}

.wait_text{
  font-size: 1.5rem;
  margin: 1em auto;
  text-align: center;
  color: #464545;
}

</style>