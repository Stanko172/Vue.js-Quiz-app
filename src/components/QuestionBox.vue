<template>
        <div>
            <b-jumbotron bg-variant="info" text-variant="dark" border-variant="dark">

                <template v-slot:lead>
                    {{ currentQuestion.question }}
                </template>

                <hr class="my-4">

                <b-list-group>
                    <b-list-group-item v-for="(answer, index) in answers" :key="index" @click="selectedAnswer(index)" 
                    :class="[!answered && selectedIndex === index ? 'selected' : 
                        answered && correctIndex === index ? 'correct':
                        answered &&  correctIndex !== index && selectedIndex === index? 'incorrect': ''
                    ]">
                        {{ answer }}
                    </b-list-group-item>
                </b-list-group>
                <div class="buttons">
                <b-button @click="next" variant="danger" href="#">Sljedeće</b-button>
                <b-button variant="success" @click="submitAnswer" :disabled="selectedIndex === null || answered">Pošalji</b-button>
                </div>
            </b-jumbotron>
        </div>
</template>

<script>
    export default{
        props:{
            currentQuestion: Object,
            next: Function,
            increment: Function
        },
        data(){
            return{
                 selectedIndex: null,
                 correctIndex: null,
                 answered: false
            } 
        },
        watch: {
            currentQuestion:{
                immediate: true,
                handler(){
                    this.selectedIndex = null,
                    this.answered = false,
                    this.checkAnswers()
                }
                
            }
        },
        methods:{
            selectedAnswer(index){
                this.selectedIndex = index;
                console.log(index);      
            },
            checkAnswers(){
               this.correctIndex = this.answers.indexOf(this.currentQuestion.correct_answer);
               console.log(this.correctIndex);
            },
            submitAnswer(){
                let isCorrect = false;
                if(this.selectedIndex === this.correctIndex){
                    isCorrect = true;
                }
                this.answered = true;

                this.increment(isCorrect);
            }
        },
        computed: {
            answers(){
                let answers = [...this.currentQuestion.incorrect_answers];
                 function getRandomInt(max) {
                    return Math.floor(Math.random() * Math.floor(max));
                    }
                answers.splice(getRandomInt(answers.length),0,this.currentQuestion.correct_answer)
                return answers
            }
        },
        mounted(){
            console.log(this.currentQuestion);
        }
    }
</script>

<style>
    .buttons{
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        justify-items: center;
        margin-top: 1.5em;
    }

    .btn{
        width: 20em;
        font-size: 1.2em;
        padding: 0.5em;
        border-radius: 0.5em;
    }

    .list-group-item:hover{
        background: #f4f4f4;
        cursor: pointer;
    }

    .selected{
        background: darkgray;
    }

    .correct{
        background: green;
    }

    .incorrect{
        background: red;
    }

    @media only screen and (max-width: 992px) {
        .btn{
        width: 5em;
        font-size: 1.2em;
        padding: 0.5em;
        border-radius: 0.5em;
    }
    }
</style>