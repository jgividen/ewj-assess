<template>
    <div class="p-4 mx-auto" v-show="isVisible">
        <div v-if="currentIndex !== null && currentIndex + 1 <= questions.length">
            <TransitionGroup tag="div" class="relative">
                <div v-for="question, index in questions" :key="question.Id" v-show="index === currentIndex" class="absolute top-0">
                    <div class="flex">
                        <div class="mr-2">{{ index + 1 }}.</div>
                        <TypeQuestion :question="question" v-model="question.Answer" @update:modelValue="nextQuestion"></TypeQuestion> 
                    </div>
                </div>
            </TransitionGroup>
        </div>
        <div v-else>
            <div class="mb-4 text-4xl flex justify-center">Type {{ enneagramType }}</div>
            <TypeDescription :enneagramType="enneagramType"></TypeDescription>
        </div>
    </div>
</template>

<script lang="ts" setup>
import { ref, onMounted, computed } from 'vue';
import allQuestions from '@/utils/questions.json';
import TypeQuestion from '@/components/TypeQuestion.vue';
import TypeDescription from '@/components/TypeDescription.vue';

interface Question {
    Id: number;
    Question: string;
    Type: number;
    Answer: number;
}

const questions = ref<Question[]>([]);
const currentIndex = ref<number | null>(null);
const isVisible = ref(false);

const nextQuestion = () => {
    if (currentIndex.value === null) {
        return;
    }
    currentIndex.value++;
}

const shuffle = (array: Question[]) => {
    array.sort(() => Math.random() - 0.5);
}

const enneagramType = computed(() => {
    const scores: any = {};
    questions.value.forEach((question) => {
        if (question.Answer) {
            if (!scores[question.Type]) {
                scores[question.Type] = 0;
            }
            scores[question.Type] += question.Answer;
        }
    });
    const enneagramTypes = Object.keys(scores);
    if (enneagramTypes.length === 0) {
        return 0;
    }
    return enneagramTypes.reduce((maxTypeNum, currentTypeNum) => {
        if (scores[currentTypeNum] > scores[maxTypeNum]) {
            maxTypeNum = +currentTypeNum;
        }
        return maxTypeNum;
    }, +enneagramTypes[0]);
});

onMounted(() => {
    const urlParams = new URLSearchParams(location.search);
    if (urlParams.get('ea') === 'true') {
        isVisible.value = true;
    } else {
        return;
    }
    const numberPerType = 10;
    const allQuestionsWithIds = allQuestions.map((question: Question, index: number) => {
        return {
            ...question,
            Id: index + 1,
            Answer: 0,
            Type: question.Type,
        }
    });

    for (let i = 0; i < 9; i++) {
        const enneagramType = i + 1;
        const allQuestionsForType = allQuestionsWithIds.filter((question: Question) => {
            return question.Type === enneagramType;
        });
        const questionsForType = [];
        while (questionsForType.length < numberPerType) {
            const questionIndex = Math.floor(Math.random() * allQuestionsForType.length);
            const question = allQuestionsForType[questionIndex];
            questionsForType.push(question);
            allQuestionsForType.splice(questionIndex, 1);
        }
        questions.value.push(...questionsForType);
    }
    shuffle(questions.value);
    currentIndex.value = 0;
});
</script>

<style scoped>
.v-enter-active,
.v-leave-active {
  transition: opacity 0.8s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>