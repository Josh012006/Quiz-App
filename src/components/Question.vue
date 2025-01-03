<template>
    <form @submit.prevent="confirmAnswer">
        <h4>{{ question.question }}</h4>
        <label v-for="choice in question.choices"  :key="choice" :for="choice"><input type="radio" name="answer" :id="choice" :value="choice" v-model="answer"> {{ choice }}</label>
        <br>
        <button :disabled="answer === ''" class="confirm">Question suivante</button>
    </form>
</template>

<script setup>

import { ref, watch } from "vue"

const emits = defineEmits(['answer'])

const props = defineProps({
    question: Object
})

const answer = ref('');

const confirmAnswer = () => {

    const correct = answer.value == props.question.correct_answer
    emits('answer', correct);
}

watch(() => props.question, () => {
    answer.value = '';
})

</script>

<style>

.confirm {
    background-color: lightseagreen;

    padding: 10px;
    margin: 20px;

    border-width: 0px;
    border-radius: 7px;
    cursor: pointer;

    color: white;

    max-width: 150px;

    position: relative;

    left: 60%;
}

.confirm:disabled {
    background-color: cadetblue;
}

form {
    display: flex;
    flex-direction: column;

    align-items: start;

    padding: 30px;
}

</style>