<template>
    <div class="question_card">
        <h3>{{question.question}}</h3>

        <div class="options">
            <button v-for="(option,index) in question.answers" :key="index" @click="handleAnswering(index)" :class="determineAnswerClass(index)">
                {{option}}
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: "QuizQuestion",
    data(){
        return {
            showAnswers: false
        }
    },
    props: {
        question: {
            question: String,
            answers: [String],
            correctAnswerIndex: Number
        },
    },
    watch:{
        question: function(newValue, oldValue) {
            this.showAnswers = false;
        }
    },
    methods: {
        handleAnswering(index){
            this.showAnswers = true;
            this.$emit('answerEvent',index)
        },

        determineAnswerClass(index){
            if(!this.showAnswers) return ""
            return index === this.question.correctAnswerIndex ? 'correct' : 'wrong'
        }
    },
}
</script>

<style scoped>

.question_card{
    font-family: 'Montserrat', sans-serif;
}

.question_card h3{
    text-align: center;
    color: black;
    font-size: 1.4rem;
}

.question_card h3:before{
    content: "Q. ";
}

.options{
    display: grid;
    grid-template-columns: repeat(2, minmax(100px, 1fr));
    grid-gap: 0.5em;
    margin: 15px 0;
}

.options button{
    border-radius: 15px;
    background-color: #C8C8C8;
    transition: background-color 0.2s ease-in-out, color 0.2s ease-in-out;
}

.options button:hover{
    background-color: #363636;
    color: white;
}

.correct,
.correct:hover{
    background-color: #91FF6A !important;
    color: black !important;
    font-weight: bold;
}



.wrong,
.wrong:hover{
    background-color: #FF8888 !important;
    color: black !important;
}

@media (max-width: 450px){
    .options{
        display: flex;
        flex-direction: column;
    }

    .options button{
        margin: 5px 0;
    }
}

/* this is just to remove the hover effect caused after clicking an answer button in android devices */
@media (hover: none) {
  .options button:hover { color: black; background: #C8C8C8; }
}

</style>