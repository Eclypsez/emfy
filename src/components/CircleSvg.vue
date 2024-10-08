<template>
    <svg width="20" height="20">
        <circle cx="10" cy="10" r="5" :fill="circleColor" />
    </svg>
</template>

<script setup>
import { computed, defineProps } from 'vue';

const props = defineProps({
    color: {
        type: Number,
        required: true
    },
})

console.log(props.color);


const circleColor = computed(() => {
    switch (props.color) {
        case 1727729940:
            return 'red';
        case 1727816340:
            return 'green';
        case 1727902740:
            return 'yellow';
        default:
            return 'gray';
    }
});
</script>
