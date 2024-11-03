<script>
import WorkoutListItem from './WorkoutListItem.vue';
  export default {
    name: "WorkoutList",
    components: {
        WorkoutListItem,
    },
    props: {
        list: {
            type: Array,
            required: true,
        },
        isButton: {
            type: Boolean,
            default: false,
        },
    },
  }
</script>

<template>
    <ul class="list-group list-group-flush mb-3">
        <workout-list-item 
            v-for="item in list" 
            :key="item.title" 
            :item="item"
            :isButton="isButton">
        </workout-list-item>
    </ul>
</template>

<style scoped>

</style>
