<script>

import WeightExercise from '@/models/weight-exercise';
import CardioExercise from '@/models/cardio-exercise';
    const ExerciseItemCard = {
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
        components: {
            WeightExercise,
            CardioExercise,
            ExerciseItemCard,
        },
    }
</script>

<template>

</template>

<style scoped>

</style>
