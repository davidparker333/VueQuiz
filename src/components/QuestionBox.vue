<template>
    <div class="row d-flex justify-content-center">
        <div class="col-6">
            <b-jumbotron>
            <template #lead>
                <span v-html="question.question"></span>
            </template>

            <hr class="my-4">

            <b-list-group class="my-4 px-5">
                <b-list-group-item @click="selectAnswer(index)" v-for="(answer, index) in shuffledAnswers" :key="index" v-html="answer" :class="answerClass(index)"></b-list-group-item>
            </b-list-group>

            <b-button @click="submitAnswer" :disabled="selectedIndex === null || submitted" variant="primary" class="mx-1">Submit</b-button>
            <b-button @click="next" variant="success" class="mx-1">Next Question</b-button>
            </b-jumbotron>
        </div>
    </div>
</template>

<script>
import _ from 'lodash'

export default {
    props: {
        question: Object,
        next: Function,
        increment: Function
    },
    data() {
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            submitted: false,
            correct_index: null
        }
    },
    computed: {
        answers() {
            let answers = [this.question.correct_answer,...this.question.incorrect_answers]
            return answers
        }
    },
    watch: {
        question: {
            immediate: true,
            handler() {
                this.submitted = false
                this.selectedIndex = null
                this.shuffleAnswers()
            }
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index
        },
        shuffleAnswers() {
            let answers = [this.question.correct_answer,...this.question.incorrect_answers]
            this.shuffledAnswers = _.shuffle(answers)
            this.correct_index = this.shuffledAnswers.indexOf(this.question.correct_answer)
        },
        submitAnswer() {
            let isCorrect = false
            if (this.selectedIndex === this.correct_index) {
                isCorrect = true
            }
            this.submitted = true
            this.increment(isCorrect)
        },
        answerClass(index) {
            let answerClass = ""
            if (!this.submitted && this.selectedIndex === index) {
               answerClass = 'selected'
            } else if (this.submitted && this.correct_index === index) {
                answerClass = 'correct'
            } else if (this.submitted && this.correct_index !== index && this.selectedIndex === index) {
                answerClass = 'incorrect'
            }
            return answerClass
        }
    }
}
</script>

<style scoped>
.list-group-item:hover {
    background-color: #eee;
    cursor: pointer;
}

.selected {
    background-color: lightblue;
}

.correct {
    background-color: lightgreen;
}

.incorrect {
    background-color: red;
}
</style>