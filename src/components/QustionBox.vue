<template>
<div class="question-box-container\">
 <b-jumbotron >

  <template v-slot:lead>
   {{ tanya.question }}
  </template>

  <hr class="my-4">
  <b-list-group>
   <b-list-group-item 
    v-for="(answer, index) in shuffleAnswer" 
    :key="index"
    @click="selectAnswer(index)"
    :class="answerClass(index)"
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
  <b-button @click="next" variant="success" :disabled="!answered">
   Next
  </b-button>
 </b-jumbotron>
</div>
</template>
<script src='https://cdn.jsdelivr.net/g/lodash@4(lodash.min.js+lodash.fp.min.js)'></script>
<script>
	
var fp = require('lodash/fp');
	export default{

		props: {
			tanya: Object,
			next: Function,
			increment: Function,
			
		},
		data: function(){

			return{					
				selectedIndex: null,
				correctIndex:null,
				shuffleAnswer: [],
				answered: false
			}

		},
		computed: {
			answers() {
				let answers = [...this.tanya.incorrect_answers]
				answers.push(this.tanya.correct_answer)
				return answers
			}
		},
		watch:{
			tanya:{
				immediate: true,
				handler(){
					this.selectedIndex = null
					this.answered = false
					this.shuffled()
				}
			}			
		},
		methods: {
			selectAnswer(index){
				this.selectedIndex=index
			},
			submitAnswer(){
				let isCorrect = false
				if(this.selectedIndex === this.correctIndex){
					isCorrect = true
				}
				this.answered = true
				this.increment(isCorrect)
			},
			shuffled(){
				let answers = [...this.tanya.incorrect_answers, this.tanya.correct_answer]
				this.shuffleAnswer = fp.shuffle(answers)
				this.correctIndex = this.shuffleAnswer.indexOf(this.tanya.correct_answer)
			},
			answerClass(index){
				let answerClass = ''
				if(!this.answered && this.selectedIndex === index){
					answerClass = 'selected'
				}else if(this.answered && this.correctIndex === index){
					answerClass = 'correct'
				}else if(this.answered && 
					this.selectedIndex === index && 
					this.correctIndex !== index
					){
					answerClass = 'incorrect'
				}
				return answerClass
			},
			alert(){
				let refresh = this.pIndex
			}
		},

	}
</script>
<style scoped>
	.list-group {
		margin-bottom: 15px;
	}
	.list-group-item:hover {
		background-color: #EEE;
		cursor: pointer;
	}
	.btn {
		margin: 0 5px;
	}
	.selected{
		background-color: lightblue;
		color: orange;
	}
	.correct{
		background-color: lightgreen;
	}
	.incorrect{
		background-color: red;
	}
</style>