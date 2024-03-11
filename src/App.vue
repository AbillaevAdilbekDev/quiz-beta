<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  {
	question: 'Jer Qanday formada?',
	answer: 0,
	options: [
		'Shar',
		'Kub',
		'Sfera'
	],
	selected: null
  },
  {
	question: "Sonay eki sandi tabin' kobeymesi,qosindisi kvadrati ten' bolsin.Ms a*a=a+a=a^2",
	answer: 2,
	options: [
		'1',
		'0',
		'2'
	],
	selected: null
  },
  {
	question: "x + 3 = x ten'lemeni sheshin'",
	answer: 1,
	options: [
		'3',
		'boskoplik',
		'1.5'
	],
	selected: null
  }
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
	let value = 0
	questions.value.map(q => {
		if (q.selected != null && q.answer == q.selected) {
			console.log('correct');
			value++
		}
	})
	return value
})

const getCurrentQuestion = computed(() => {
	let question = questions.value[currentQuestion.value]
	question.index = currentQuestion.value
	return question
})

const SetAnswer = (e) => {
	questions.value[currentQuestion.value].selected = e.target.value
	e.target.value = null
}

const NextQuestion = () => {
	if (currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++
		return
	}
	
	quizCompleted.value = true
}
</script>

<template>
	<main class="app">
		<h1 class="white">Viktorina</h1>
		
		<section class="quiz" v-if="!quizCompleted">
			<div class="quiz-info">
				<span class="question">{{ getCurrentQuestion.question }}</span>
				<span class="score white">Ochko {{ score }}/{{ questions.length }}</span>
			</div>
			
			<div class="options">
				<label 
					v-for="(option, index) in getCurrentQuestion.options" 
					:for="'option' + index" 
					:class="`option ${
						getCurrentQuestion.selected == index 
							? index == getCurrentQuestion.answer 
								? 'correct' 
								: 'wrong'
							: ''
					} ${
						getCurrentQuestion.selected != null &&
						index != getCurrentQuestion.selected
							? 'disabled'
							: ''
					}`">
					<input 
						type="radio" 
						:id="'option' + index" 
						:name="getCurrentQuestion.index" 
						:value="index" 
						v-model="getCurrentQuestion.selected" 
						:disabled="getCurrentQuestion.selected"
						@change="SetAnswer"
						class="white" 
					/>
					<span>{{ option }}</span>
				</label>
			</div>
			
			<button 
				@click="NextQuestion" 
				:disabled="!getCurrentQuestion.selected">
				{{ 
					getCurrentQuestion.index == questions.length - 1 
						? 'Tamamlaw' 
						: getCurrentQuestion.selected == null
							? 'Juwapti sayla'
							: 'Keyingi Soraw'
				}}
			</button>
		</section>

		<section v-else>
			<h2 class="white">Sen viktorinani tamamladin'</h2>
			<p class="white">Senin' jiynag'an ochkon' {{ score }}/{{ questions.length }}</p>
			<a class="btn btn-primary center" href="/">
				Qaytadan baslaw
			</a>
		</section>
	</main>
</template>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'Montserrat', sans-serif;
}

body {
	background-color: #271c36;
	color: #FFF;
}
.app {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 2rem;
	height: 100vh;
}

h1 {
	font-size: 2rem;
	margin-bottom: 2rem;
}

.quiz {
	background-color: #382a4b;
	padding: 1rem;
	width: 100%;
	max-width: 640px;
}

.quiz-info {
	display: flex;
	justify-content: space-between;
	margin-bottom: 1rem;
}
.white {
	color: #fff;
}
.quiz-info .question {
	color: #8F8F8F;
	font-size: 1.25rem;
}

.quiz-info.score {
	color: #FFF;
	font-size: 1.25rem;
}
.center {
	width: 30%;
	margin-left: 35%;
	margin-right: 35%;
}
.options {
	margin-bottom: 1rem;
}

.option {
	padding: 1rem;
	display: block;
	background-color: #fff;
	margin-bottom: 0.5rem;
	border-radius: 0.5rem;
	cursor: pointer;
}

.option:hover {
	background-color: #ccc;
}

.option.correct {
	background-color: #2cce7d;
}

.option.wrong {
	background-color: #ff5a5f;
}

.option:last-of-type {
	margin-bottom: 0;
}

.option.disabled {
	opacity: 0.5;
}

.option input {
	display: none;
}

button {
	appearance: none;
	outline: none;
	border: none;
	cursor: pointer;
	padding: 0.5rem 1rem;
	background-color: #2cce7d;
	color: #2d213f;
	font-weight: 700;
	text-transform: uppercase;
	font-size: 1.2rem;
	border-radius: 0.5rem;
}

button:disabled {
	opacity: 0.5;
}

h2 {
	font-size: 2rem;
	margin-bottom: 2rem;
	text-align: center;
}

p {
	color: #8F8F8F;
	font-size: 1.5rem;
	text-align: center;
}
</style>