<script setup>
import { onMounted, ref, watch } from 'vue';

const model = defineModel({
    type: String,
    required: false,
});

const props = defineProps({
    placeholder: String,
    autoResize: {
        type: Boolean,
        default: true
    }
});

const emit = defineEmits(['update:modelValue']);

const input = ref(null);

onMounted(() => {
    if (input.value.hasAttribute('autofocus')) {
        input.value.focus();
    }

    adjustHeight()
});

watch(() => props.modelValue, () => {
    setTimeout(() => {
        adjustHeight()
    }, 10)
})

defineExpose({ focus: () => input.value.focus() });

function onInputChange($event){
    emit('update:modelValue', $event.target.value)
}

function adjustHeight(){
    if(props.autoResize){
        input.value.style.height = 'auto';
        input.value.style.height = (input.value.scrollHeight + 1) + 'px';
    }
}
</script>

<template>
    <textarea
        class="border-gray-300 dark:border-gray-700 dark:bg-gray-900 dark:text-gray-300 focus:border-indigo-500 dark:focus:border-indigo-600 focus:ring-indigo-500 dark:focus:ring-indigo-600 rounded-md shadow-sm"
        v-model="model"
        @input="onInputChange"
        ref="input"
        :placeholder="placeholder"
    ></textarea>
</template>
