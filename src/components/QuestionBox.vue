<template>
    <div class="question-box-container">
            <b-jumbotron>
                <template v-slot:lead>
                    {{ current_question.question }}
                </template>
                <hr class="my-4">
                <b-list-group>
                        <b-list-group-item
                                v-for="(answer, index) in shuffled_answers"
                                :key="index" @click = "selectedAnswer(index)"
                                :class="[answerClass(index)]">
                                {{ answer }}
                        </b-list-group-item>
                </b-list-group>
                <b-button variant="primary" @click="submitAnswer()" :disabled="selected_index === null || answered">
                    Submit
                </b-button>
                <b-button variant="success" @click="next">Next</b-button>
            </b-jumbotron>
    </div>
</template>

<script>
    import _ from "lodash";

    export default {
        name: "QuestionBox",
        props: {
            current_question: Object,
            next: Function,
            increment: Function
        },
        data() {
            return {
                selected_index: 0,
                correct_index: null,
                shuffled_answers: [],
                answered: false
            }
        },
        watch:{
            current_question: {
                immediate: true,
                handler(){
                    this.selected_index = null
                    this.answered = false;
                    this.shuffleAnswers()
                }
            }
        },
        methods: {
            selectedAnswer(index){
                this.selected_index = index;
            },
            submitAnswer(){
              let is_correct = false;
              if(this.selected_index === this.correct_index){
                is_correct = true;
              }
              this.answered = true;
              this.increment(is_correct);
            },
            shuffleAnswers(){
                let answers = [...this.current_question.incorrect_answers, this.current_question.correct_answer]
                this.shuffled_answers = _.shuffle(answers)
                this.correct_index = this.shuffled_answers.indexOf(this.current_question.correct_answer)
                },
            answerClass(index){
                let class_style = "";
                if(!this.answered && this.selected_index === index){
                    class_style = "selected"
                }else if(this.answered && this.correct_index === index){
                    class_style = "correct"
                }else if(this.answered && this.selected_index === index && this.correct_index !== index){
                    class_style = "incorrect"
                }
                return class_style;
            }
        },
    }
</script>

<style scoped>
    .list-group{
        margin-bottom: 15px;
    }
    .list-group-item:hover{
        background: #EEE;
        cursor: pointer;
    }
    .btn{
        margin: 0 5px;
    }
    .selected{
        background-color: lightblue;
    }
    .correct{
        background-color: lightgreen;
    }
    .incorrect{
        background-color: red;
    }
</style>
