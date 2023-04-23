<template>
    <div>
        <div class="mb-2">{{ question.Question }}</div>
        <div>
            <div class="question-answer">
                <input type="radio" :id="`answer-1-${question.Id}`" :name="`answer${question.Id}`" value="1" v-model="answer">
                <label :for="`answer-1-${question.Id}`">Almost Never</label><br>
            </div>
            <div class="question-answer">
                <input type="radio" :id="`answer-2-${question.Id}`" :name="`answer${question.Id}`" value="2" v-model="answer">
                <label :for="`answer-2-${question.Id}`">Rarely</label><br>
            </div>
            <div class="question-answer">
                <input type="radio" :id="`answer-3-${question.Id}`" :name="`answer${question.Id}`" value="3" v-model="answer">
                <label :for="`answer-3-${question.Id}`">Sometimes</label><br>
            </div>
            <div class="question-answer">
                <input type="radio" :id="`answer-4-${question.Id}`" :name="`answer${question.Id}`" value="4" v-model="answer">
                <label :for="`answer-4-${question.Id}`">Frequently</label><br>
            </div>
            <div class="question-answer">
                <input type="radio" :id="`answer-5-${question.Id}`" :name="`answer${question.Id}`" value="5" v-model="answer">
                <label :for="`answer-5-${question.Id}`">Almost Always</label><br>
            </div>
        </div>
    </div>
</template>

<script lang="ts" setup>
import { defineProps, defineEmits, ref, watch } from 'vue';

interface Question {
    Id: number;
    Question: string;
    Type: number;
}

const props = defineProps<{
    question: Question,
    modelValue: number | null,
}>();

const answer = ref();

const emit = defineEmits([ 'update:modelValue' ]);

watch(answer, () => {
    emit('update:modelValue', answer.value ? +answer.value : null);
});
</script>

<style scoped>
.question-answer {
    @apply flex items-center;
}
.question-answer input {
    @apply cursor-pointer;

    margin-right: 8px;
    height: 20px;
    width: 20px;
}

.question-answer label {
    @apply cursor-pointer;
}
</style>