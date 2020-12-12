<template>
  <div class="d-flex justify-content-center">
      
    <div v-if="showScore">
        <b-card
        title="Results"
        style="max-width: 20rem;"
        >
        You Scored {{score}} of {{questions.length}}
        </b-card>
    </div>
    <div class="card-q" v-else>
    <b-card
    title="Quiz ID"
    style="max-width: 20rem;"
    class="mb-2"
  
  >
   <b-progress
        variant="warning"
        :max="30"
        :value="countDown"
        height="4px"
      ></b-progress>
     <b-card-text>
      Question No.{{currentQuestion + 1}} of {{questions.length}}
    </b-card-text>
     <b-card-text>
      {{countDown}}
    </b-card-text>
    <b-card-text>
      {{questions[currentQuestion].questionText}}
    </b-card-text>
    <div class="answer-section">
    <b-button :key="index" v-for="(option, index) in questions[currentQuestion].answerOptions" @click="handleAnswerClick(option.isCorrect)" class="ans-option-btn" variant="primary">{{option.answerText}}</b-button>
    </div>
  </b-card>
  </div>
  </div>
</template>

<script>
export default {
    data(){
        return {
            currentQuestion: 0,
            showScore: false,
            score:0,
            countDown : 30,
            timer:null,

            questions : [
		{
			questionText: 'Which one is used for two-way binding?',
			answerOptions: [
				{ answerText: 'v-on', isCorrect: false },
                { answerText: 'v-bind', isCorrect: false },
				{ answerText: 'v-model', isCorrect: true },
				{ answerText: 'v-if', isCorrect: false },
                
			
			],
		},
		{
			questionText: 'Who is the creator of vueJS ?',
			answerOptions: [
				{ answerText: 'Jeff Bezos', isCorrect: false },
				{ answerText: 'Elon Musk', isCorrect: false },
				{ answerText: 'Evan You', isCorrect: true },
				{ answerText: 'Tony Stark', isCorrect: false },
			],
		},
		{
			questionText: 'Vue is used in the backend. - True or False?',
			answerOptions: [
				{ answerText: 'True', isCorrect: false },
				{ answerText: 'False', isCorrect: true },
			],
		},
		{
			questionText: 'Which version of Vue is Launched on 2020?',
			answerOptions: [
				{ answerText: 'Vue 2', isCorrect: false },
				{ answerText: 'Vue 1', isCorrect: false },
				{ answerText: 'Vue 4', isCorrect: false },
				{ answerText: 'Vue 3', isCorrect: true },
			],
        },
        {
			questionText: 'Is vue an OpenSource Library?',
			answerOptions: [
				{ answerText: 'True', isCorrect: true },
				{ answerText: 'False', isCorrect: false },
			],
        },
        {
			questionText: 'Which of the following is a Full Javascript Frramework',
			answerOptions: [
				{ answerText: 'Vue', isCorrect: false },
				{ answerText: 'node', isCorrect: false },
				{ answerText: 'react', isCorrect: false },
				{ answerText: 'Angular', isCorrect: true },
			],
        },
        {
			questionText: 'Composition API can be used on which version?',
			answerOptions: [
				{ answerText: 'Vue 5', isCorrect: false },
				{ answerText: 'Vue 2 Only', isCorrect: false },
				{ answerText: 'Vue 3 Only', isCorrect: false },
				{ answerText: 'Both Vue 2 and Vue 3', isCorrect: true },
			],
		},
    ],
    

        }
    },

    methods:{
        fetchQuiz(){
            this.$http.get(
                'http://localhost:1337/quizes/?quizID=test1'
            ).then( res => {
                console.log('response',res)
            }).catch(err=>{
                console.log(err)
            })
        },
        handleAnswerClick(isCorrect){
            clearTimeout(this.timer);
            let nextQuestion = this.currentQuestion + 1;
            if(isCorrect){
                this.score = this.score + 1;
            }
            if(nextQuestion < this.questions.length){
            this.currentQuestion = nextQuestion;
            // this.$store.state.questionAttended = this.currentQuestion;
            // localStorage.setItem('qattended', this.currentQuestion)

            this.countDown = 30;
            this.countDownTimer();
            }
            else{
                // localStorage.removeItem('qattended')
                this.showScore = true;
                // localStorage.setItem('testComplete',this.showScore)
            }

        },
        countDownTimer() {
                if(this.countDown > 0) {
                    this.timer = setTimeout(() => {
                        this.countDown -= 1
                        this.countDownTimer()
                    }, 1000)
                }
                else{
                    this.handleAnswerClick(false)
                }
            }
    },
     created() {
        //  alert(this.$store.state.questionAttended)
        //    this.showScore = localStorage.getItem('testComplete') || false
        //    this.currentQuestion = localStorage.getItem('qattended') || 0
           this.countDownTimer()
           this.fetchQuiz()
        }
}
</script>

<style scoped>
.card{
    min-width: 100%;
    border-radius: 15px;
    padding: 20px;
    box-shadow: 10px 10px 42px 0px rgba(0, 0, 0, 0.75);
}
.card-q{
    min-width: 60%;
}
.ans-option-btn{
    min-width: 50%;
    font-size: 16px;
    color: #ffffff;
    align-items: center;
    cursor: pointer;
    margin-bottom: 5px;


}
.answer-section {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.timer-text {
  background: rgb(230, 153, 12);
  padding: 15px;
  margin-top: 20px;
  margin-right: 20px;
  border: 5px solid rgb(255, 189, 67);
  border-radius: 15px;
  text-align: center;
}
/* .ans-option-btn {
  width: 100%;
  font-size: 16px;
  color: #ffffff;
  background-color: #252d4a;
  border-radius: 15px;
  display: flex;
  padding: 5px;
  justify-content: flex-start;
  align-items: center;
  border: 5px solid #234668;
  cursor: pointer;
} */

</style>
