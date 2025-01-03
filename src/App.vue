<template>
    <h3>{{ title }}</h3>

    <Progress :progress="progress" />

    <Question v-if="questions.length !== 0 && answered != questions.length"  :question="questions[answered]" @answer="checkResult" />
    <p v-if="questions.length !== 0 && answered == questions.length">{{ result }}</p>
</template>

<script setup>
import { ref, computed, onMounted, watchEffect } from 'vue'
import Progress from './components/Progress.vue'
import Question from './components/Question.vue'


const infos = ref(null);
const answered = ref(0);
const score = ref(0);

const title = computed(() => {
    return (infos.value) ? infos.value.title : '';
});

const questions = computed(() => {
    return (infos.value) ? infos.value.questions : [];
})

const progress = computed(() => {
    const total = (infos.value) ? questions.value.length : 0;

    return (total !== 0) ? (answered.value * 100 / total) : 0;
})


onMounted(() => {
    fetch("./quiz.json")
    .then(response => response.json())
    .then(info => {infos.value = info})
})


const checkResult = (correct) => {
    
    if(correct) {
        score.value++;
    }

    answered.value++;

}

const result = computed(() => {
    if(infos.value) {
        return (score.value >= infos.value.minimum_score) ? infos.value.success_message :infos.value.failure_message;
    }
    else {
        return '';
    }
});




</script>

<style>

body {
    padding: 100px;
}

</style>