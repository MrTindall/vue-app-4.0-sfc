<script>
import AppModal from './components/AppModal.vue';
import CardioExerciseCard from './components/CardioExerciseCard.vue';
import ExerciseItemCard from './components/ExerciseItemCard.vue';
import ExerciseList from './components/ExerciseList.vue';
import WeightExerciseCard from './components/WeightExerciseCard.vue';
import WorkoutList from './components/WorkoutList.vue';
import WorkoutListItem from './components/WorkoutListItem.vue';
import Exercise from './models/exercise';
import CardioExercise from './models/cardio-exercise';
import WeightExercise from './models/weight-exercise';
export default {
    components: {
        ExerciseList,
        WorkoutList,
        ExerciseItemCard,
        AppModal,
        WorkoutListItem,
        CardioExerciseCard,
        Exercise,
        WeightExerciseCard,
        CardioExercise,
        WeightExercise
    },
    // data: all the data for the app, must return an object
    data: function () {
        return {
            exerciseType: Object.freeze({
                WEIGHT: 'Weight',
                CARDIO: 'Cardio',
            }),
            selectedExerciseType: null,
            selectedWorkout: null,
            tempBuilderWorkoutName: '',

            exercise: {
                name: '',
                amount: 0,
                sets: 3,
                remove: false,
                isActive: false,
            },
            newWorkout: {
                title: '',
            },
            workouts: [
                {
                    title: 'Push',
                    exercises: [
                        new CardioExercise('Run', 10, 1),
                        new WeightExercise('Arnold Press', 185, 4),
                        new WeightExercise('Bench Press', 185, 4),
                        new WeightExercise('Incline DB Press', 50, 4),
                        new WeightExercise('Pushups', 0, 4),
                        new WeightExercise('Cable Crossover', 60, 4),
                        new WeightExercise('Shoulder Press', 40, 4),
                        new WeightExercise('Lateral Raises', 15, 4),
                        new WeightExercise('Front Raise', 15, 4),
                        new WeightExercise('Tricep Extensions', 40, 4),
                        new WeightExercise('Skull Crushers', 60, 4),
                    ]
                },
                {
                    title: 'Pull',
                    exercises: [
                        new CardioExercise('Run', 10, 1, 1),
                        new WeightExercise('Pullups', 0, 4),
                        new WeightExercise('DB Row', 185, 4),
                    ]
                },
                {
                    title: 'Legs',
                    exercises: [
                        new CardioExercise('Run', 10, 12, 1),
                        new WeightExercise('Deadlift', 335, 3),
                        new WeightExercise('Squat', 235, 3),
                        new WeightExercise('Lunges', 40, 4),
                    ]
                },

            ],
            builderTempList: [],
            tempWorkout: {
                title: '',
                exercises: []
            },
        }
    },

    // methods: usually "events" triggered by v-on:
    methods: {
        chooseWorkout() {
            this.tempWorkout = {
                title: this.selectedWorkout.title,
                exercises: [...this.selectedWorkout.exercises]
            };
            this.tempWorkout.exercises.forEach((exercise) => {
                exercise.isActive = true;
                console.log(exercise)
            })

        },

        createWorkout() {
            if (this.newWorkout.title && this.builderTempList.length > 0) {
                if (this.selectedExerciseType === this.exerciseType.WEIGHT) {
                    this.workouts.push({
                        title: this.newWorkout.title,
                        exercises: this.builderTempList.map(exercise =>
                            new WeightExercise(exercise.name, exercise.amount, exercise.sets, exercise.isActive = false)
                        )
                    });
                } else if (this.selectedExerciseType === this.exerciseType.CARDIO) {
                    this.workouts.push({
                        title: this.newWorkout.title,
                        exercises: this.builderTempList.map(exercise =>
                            new CardioExercise(exercise.name, exercise.amount, exercise.isActive = false)
                        )
                    });
                }

                // Reset fields after adding workout
                this.newWorkout = { title: '' };
                this.builderTempList = [];
            } else {
                alert("Please provide a valid workout name and at least one exercise.");
            }
        },
        addExercise() {
            this.exerciseList.push(this.exercise);
            this.exercise = {
                name: '',
                amount: 0,
                sets: 3,
                remove: false,
                isActive: false,
            };
        },
        addBuildExercise() {
            let newExercise;
            // Check if the current exercise is Cardio or Weight
            if (this.selectedExerciseType === this.exerciseType.CARDIO) {
                newExercise = new CardioExercise(this.exercise.name, this.exercise.amount, this.exercise.sets);
            } else if (this.selectedExerciseType === this.exerciseType.WEIGHT) {
                newExercise = new WeightExercise(this.exercise.name, this.exercise.amount, this.exercise.sets);
            }

            // Push the new exercise to the list
            this.builderTempList.push(newExercise);

            // Reset the exercise input fields
            this.exercise = {
                name: '',
                amount: 0,
                sets: 3,
                remove: false,
                isActive: false,
            };
        },
        setTempBuilderWorkoutName() {
            this.tempBuilderWorkoutName = this.exercise.workoutName
        },
        addBuildExerciseToExerciseList() {
            this.builderTempList.forEach((item) => {
                const exerciseToAdd = {
                    name: item.name,
                    amount: item.amount,
                    sets: item.sets,
                    remove: false,
                    isActive: false,
                };
                this.workouts.push(exerciseToAdd);
            });


            this.builderTempList = [];
            this.exercise = {
                name: '',
                amount: 0,
                sets: 3,
                remove: false,
                isActive: false,
            };
        },
        finishWorkout() {
            this.tempWorkout =
            {
                title: '',
                exercises: []
            },

                this.selectedWorkout = null; // Reset selectedWorkout
        },
    },

    // computed: values that are updated and cached if dependencies change
    computed: {

        todoList() {
            return this.tempWorkout?.exercises.filter((exercise) => exercise.sets > 0 && !exercise.remove && exercise.isActive === true) ?? [];
        },

        completeList() {
            return this.tempWorkout?.exercises.filter((exercise) => exercise.sets <= 0 && exercise.isActive === true || exercise.remove && exercise.isActive === true) ?? [];
        },

        removeExerciseList() {
            return this.tempWorkout?.exercises.filter((exercise) => exercise.sets > 0 && exercise.remove) ?? [];
        },

        isActiveList() {

            return this.tempWorkout?.exercises.filter((exercise) => exercise.remove && !exercise.isActive) ?? [];
        },
    },

    //mounted:  called after the instance has been mounted,
    mounted: function () {

    },

    // watch:   calls the function if the value changes
    watch: {

    }
}
</script>

<template>
    <div class="container-fluid">
        <div class="container-fluid my-3 contentContainer">
            <ul class="nav nav-tabs workoutTabs">
                <li class="nav-item">
                    <a class="nav-link active" data-bs-toggle="tab" href="#workoutTab">WORKOUT</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" data-bs-toggle="tab" href="#completedTab">COMPLETED</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" data-bs-toggle="tab" href="#builderTab">BUILDER</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" data-bs-toggle="tab" href="#historyTab">HISTORY</a>
                </li>
            </ul>

            <div class="tab-content">
                <!-- Workout Tab -->
                <div class="tab-pane fade show active" id="workoutTab">
                    <div class="card">
                        <div class="card-body m-0 px-2 py-0">
                            <div class="d-flex pt-2">
                                <div class="p-2 flex-grow-1">
                                    <h4 class="card-title text-start">{{ selectedWorkout ? selectedWorkout.title :
                                        'Start'}}</h4>
                                </div>
                                <!-- <div class="py-2 d-flex justify-content-end">
                                    <button type="button" class="addWorkout me-1" data-bs-toggle="modal"
                                        data-bs-target="#removeWorkoutModal" id="removeStart">Remove
                                    </button>
                                </div> -->
                            </div>

                            <div class="container-fluid p-0 WorkoutContent overflow-y-auto">
                                <exercise-list title="Todo List" :list="todoList" :is-button="true"></exercise-list>
                            </div>
                            <div class="py-3 d-flex justify-content-around">
                                <button type="button" class="startWorkout" data-bs-toggle="modal"
                                    data-bs-target="#startWorkoutModal">Start
                                </button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Completed Tab -->
                <div class="tab-pane fade" id="completedTab">
                    <div class="card">
                        <div class="card-body m-0 px-2 py-0">
                            <div class="d-flex pt-2">
                                <div class="p-2 flex-grow-1">
                                    <h3 class="card-title text-start">{{ selectedWorkout ? selectedWorkout.title :
                                        'Finish'}}</h3>
                                </div>
                                <!-- <div class="py-2 d-flex justify-content-end">
                                    <button type="button" class="addWorkout me-1" data-bs-toggle="modal"
                                        data-bs-target="#removeWorkoutModalComplete" id="removeComplete">Remove
                                    </button>
                                </div> -->
                            </div>
                            <div class="container-fluid p-0 WorkoutContent overflow-y-auto">
                                <exercise-list title="Complete List" :list="completeList"
                                    :is-button="true"></exercise-list>
                            </div>
                        </div>
                        <div class="py-3 d-flex justify-content-around">
                            <button type="button" class="startWorkout" data-bs-toggle="modal"
                                data-bs-target="#finishWorkoutModal">Finish
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Builder Tab -->
                <div class="tab-pane fade" id="builderTab">
                    <div class="card">
                        <div class="card-body m-0 px-2 py-0">
                            <div class="d-flex pt-2">
                                <div class="p-2 flex-grow-1">
                                    <h3 class="card-title text-start">Workout Library</h3>
                                </div>
                                <div class="py-2 d-flex justify-content-end">
                                    <button type="button" class="addWorkout me-1" data-bs-toggle="modal"
                                        data-bs-target="#deleteWorkoutModal" id="deleteComplete">Delete
                                    </button>
                                </div>
                            </div>

                            <div class="container-fluid p-0 WorkoutContent overflow-y-auto">
                                <workout-list title="workout library" :list="workouts"></workout-list>
                            </div>
                        </div>
                        <div class="py-3 d-flex justify-content-around">
                            <button type="button" class="startWorkout" data-bs-toggle="modal"
                                data-bs-target="#buildWorkoutModal">Build
                            </button>
                        </div>
                    </div>
                </div>

                <!-- History Tab -->
                <div class="tab-pane fade" id="historyTab">
                    <h3>History Content</h3>
                </div>
            </div>
        </div>


        <!-- Modal Remove Workout-->
        <app-modal id="removeWorkoutModal" title="Remove Exercise">
            <template #modal-body>
                <h5>This will remove the selected exercise(s).</h5>
                <div class="container-fluid p-0 WorkoutContentRemove overflow-y-auto">
                    <exercise-list title="Todo List" :list="removeGrtZero"></exercise-list>
                </div>
            </template>
            <template #modal-footer>
                <form>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                        <button type="button" class="btn btn-primary btn-bg-color" data-bs-dismiss="modal"
                            @click="removeExercise(removeGrtZero)">Remove</button>
                    </div>
                </form>
            </template>
        </app-modal>


        <!-- Modal Remove Workout Complete-->
        <div class="modal fade" id="removeWorkoutModalComplete" tabindex="-1"
            aria-labelledby="removeWorkoutModalCompleteLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content modal-background-color">
                    <div class="modal-header">
                        <h5 class="modal-title" id="removeWorkoutModalLabel">Remove Exercise
                        </h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <h5>This will remove the selected exercise(s).</h5>
                        <div class="container-fluid p-0 WorkoutContentRemove overflow-y-auto">
                            <exercise-list title="Remove Equals Zero" :list="removeEquZero"></exercise-list>
                        </div>
                    </div>
                    <form>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="button" class="btn btn-primary btn-bg-color" data-bs-dismiss="modal"
                                @click="setIsActive(isActiveList)">Remove</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>

        <!-- Modal Delete Builder Remove Workout -->
        <div class="modal fade" id="deleteWorkoutModal" tabindex="-1" aria-labelledby="deleteWorkoutModalLabel"
            aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content modal-background-color">
                    <div class="modal-header">
                        <h5 class="modal-title" id="deleteWorkoutModalLabel">Delete Workout
                        </h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <h5>This will delete the selected workout(s).</h5>
                        <div class="container-fluid p-0 WorkoutContentRemove overflow-y-auto">
                            <ul class="list-group list-group-flush mb-3">
                                <li class="list-group-item mt-3 mx-2">
                                    <div class="d-flex flex-column flex-lg-row justify-content-between">
                                        <span class="d-flex py-1">
                                            DB Incline Press
                                        </span>
                                        <span class="d-flex justify-content-between py-1">
                                            Weight: 40 lbs
                                        </span>
                                        <span class="d-flex justify-content-between  py-1">
                                            Set: 3
                                        </span>
                                        <span></span>
                                    </div>
                                </li>
                            </ul>
                        </div>
                    </div>
                    <form>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="button" class="btn btn-primary btn-bg-color">Remove</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>

        <!-- Modal Build Workout -->
        <!-- First Part Modal -->
        <form>
            <div>
                <div class="modal fade" id="buildWorkoutModal" tabindex="-1" aria-labelledby="buildWorkoutModalLabel"
                    aria-hidden="true">
                    <div class="modal-dialog">
                        <div class="modal-content modal-background-color">
                            <div class="modal-header">
                                <h5 class="modal-title" id="buildWorkoutModalLabel">Create Workouts Name</h5>
                                <button type="button" class="btn-close" data-bs-dismiss="modal"
                                    aria-label="Close"></button>
                            </div>
                            <div class="modal-body builder">
                                <div class="input-group mb-3">
                                    <span class="input-group-text" id="WorkoutName1">Name</span>
                                    <input type="text" class="form-control" aria-label="WorkoutName"
                                        aria-describedby="WorkoutName1" v-model="newWorkout.title">
                                </div>

                            </div>
                            <div class="modal-footer">
                                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                                <!-- Disable the button when workoutName is empty -->
                                <button class="btn btn-primary btn-bg-color" data-bs-target="#buildWorkoutModalStep2"
                                    data-bs-toggle="modal" :disabled="!newWorkout.title"
                                    @click="setTempBuilderWorkoutName()">Next</button>
                            </div>
                        </div>
                    </div>
                </div>
                <!-- Second Part Modal -->
                <div class="modal fade" id="buildWorkoutModalStep2" tabindex="-1"
                    aria-labelledby="buildWorkoutModalStep2Label" aria-hidden="true">
                    <div class="modal-dialog">
                        <div class="modal-content modal-background-color">
                            <div class="modal-header">
                                <h5 class="modal-title" id="buildWorkoutModalStep2Label">Build Workout</h5>
                                <button type="button" class="btn-close" data-bs-dismiss="modal"
                                    aria-label="Close"></button>
                            </div>
                            <div class="modal-body builder">
                                <div class="mb-3">
                                    <label for="workoutName" class="form-label">Type of Exercise</label>
                                    <select class="form-select" v-model="selectedExerciseType"
                                        aria-label="Select Exercise Type">
                                        <option v-for="(type, key) in exerciseType" :key="key" :value="type">
                                            {{ key.charAt(0) + key.slice(1).toLowerCase() }}
                                        </option>
                                    </select>
                                </div>
                                <!-- Exercise Input -->
                                <div class="d-flex justify-content-between py-1">
                                    <div class="input-group mb-3">
                                        <button type="button" class="btn btn-outline-secondary">Exercise</button>
                                        <button type="button"
                                            class="btn btn-outline-secondary dropdown-toggle dropdown-toggle-split"
                                            data-bs-toggle="dropdown" aria-expanded="false">
                                            <span class="visually-hidden"></span>
                                        </button>
                                        <ul class="dropdown-menu">
                                            <li><a class="dropdown-item" href="#">Exercise 1</a></li>
                                            <li><a class="dropdown-item" href="#">Exercise 2</a></li>
                                            <li><a class="dropdown-item" href="#">Exercise 3</a></li>
                                        </ul>
                                        <input type="text" class="form-control"
                                            aria-label="Text input with segmented dropdown button" required
                                            maxlength="25" v-model="exercise.name">
                                    </div>
                                </div>


                                <!-- Weight Input -->
                                <div class="d-flex justify-content-between py-1">
                                    <div class="input-group mb-3">
                                        <button type="button" class="btn btn-outline-secondary">Amount</button>
                                        <button type="button"
                                            class="btn btn-outline-secondary dropdown-toggle dropdown-toggle-split"
                                            data-bs-toggle="dropdown" aria-expanded="false">
                                            <span class="visually-hidden"></span>
                                        </button>
                                        <ul class="dropdown-menu">
                                            <li><a class="dropdown-item" href="#">Weight 1</a></li>
                                            <li><a class="dropdown-item" href="#">Weight 2</a></li>
                                            <li><a class="dropdown-item" href="#">Weight 3</a></li>
                                        </ul>
                                        <input type="number" class="form-control"
                                            aria-label="Number input with segmented dropdown button" required
                                            maxlength="5" v-model="exercise.weight">
                                    </div>
                                </div>

                                <!-- Sets Input -->
                                <div class="d-flex justify-content-between py-1">
                                    <div class="input-group mb-3">
                                        <button type="button" class="btn btn-outline-secondary">Sets</button>
                                        <button type="button"
                                            class="btn btn-outline-secondary dropdown-toggle dropdown-toggle-split"
                                            data-bs-toggle="dropdown" aria-expanded="false">
                                            <span class="visually-hidden"></span>
                                        </button>
                                        <ul class="dropdown-menu">
                                            <li><a class="dropdown-item" href="#">Set 1</a></li>
                                            <li><a class="dropdown-item" href="#">Set 2</a></li>
                                            <li><a class="dropdown-item" href="#">Set 3</a></li>
                                        </ul>
                                        <input type="number" class="form-control"
                                            aria-label="Number input with segmented dropdown button" required
                                            maxlength="5" v-model="exercise.sets">
                                    </div>
                                </div>

                                <!-- Buttons for adding/removing -->
                                <div class="d-flex justify-content-end mb-3">
                                    <button type="button" class="addWorkout me-2">Remove</button>
                                    <button type="button" class="addWorkout"
                                        :disabled="!exercise.name || !selectedExerciseType"
                                        @click.prevent="addBuildExercise()">Add</button>
                                </div>

                                <!-- Workout Content -->
                                <div class="container-fluid p-0 WorkoutContentBuild overflow-y-auto">
                                    <exercise-list title="Builder List" :list="builderTempList"></exercise-list>
                                </div>
                            </div>

                            <div class="modal-footer">
                                <button class="btn btn-secondary" data-bs-target="#buildWorkoutModal"
                                    data-bs-toggle="modal">Back</button>
                                <button type="button" class="btn btn-primary btn-bg-color" data-bs-dismiss="modal"
                                    @click="createWorkout()">Build</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </form>
        <!-- Modal start Workout-->
        <app-modal title="Start Workout" id="startWorkoutModal">
            <template #modal-body>
                <form>
                    <div class="mb-3">
                        <label for="workoutName" class="form-label">Workout Name</label>
                        <select class="form-select" v-model="selectedWorkout" aria-label="workoutName">
                            <option v-for="workout in workouts" :key="workout.title" :value="workout">
                                {{ workout.title }}
                            </option>
                        </select>
                    </div>
                </form>
            </template>
            <template #modal-footer>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary btn-bg-color" data-bs-dismiss="modal"
                        @click="chooseWorkout()">Start
                        Workout</button>
                </div>
            </template>
        </app-modal>

        <!-- Modal finish Workout-->
        <div class="modal fade" id="finishWorkoutModal" tabindex="-1" aria-labelledby="finishWorkoutModalLabel"
            aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content modal-background-color">
                    <form @submit.prevent="finishWorkout()">
                        <div class="modal-header">
                            <h5 class="modal-title" id="finishWorkoutModalLabel">Finish Workout</h5>
                            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>
                        <div class="modal-body">
                            <div class="mb-3">
                                <h3 for="workoutName" class="form-label text-center" v-bind="selectedWorkout">
                                    Congratulations on
                                    finishing
                                    your {{ selectedWorkout ? selectedWorkout.title : '' }} Workout!</h3>
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="submit" class="btn btn-primary btn-bg-color" data-bs-dismiss="modal">Finish
                                Workout</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped></style>
