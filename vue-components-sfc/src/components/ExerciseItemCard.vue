<script>
import WeightExercise from '@/models/weight-exercise';
import CardioExercise from '@/models/cardio-exercise';

// Ask if this is the reason my list items are not displaying
    const ExerciseItem = {
        props: {
            item: {
                type: Object,
                required: true
            },
            isButton: {
                type: Boolean,
                default: false
            }
        },
    };
    export default {
        name: "ExerciseItemCard",
        extends: ExerciseItem,
        components: {
            WeightExercise,
            CardioExercise,
            ExerciseItem,
        },
    }
</script>

<template>

</template>

<style scoped>

</style>
