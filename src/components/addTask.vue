<template>
    <div class="mt-5">
        <!-- Form to Add Task -->
        <form @submit.prevent="addTask">
        <!-- Input to Add Task -->
        <input class="b-gray-400 p-3 border-2 border-blue-400 rounded-full mr-2" type="text" v-model="task">
        <!-- Button to Add Task -->
        <button class="rounded-full border-2 border-blue-500 p-3 text-white bg-blue-500 font-bold" @click="addTask" type="button">Add Task</button>
    </form>
    <!-- List of Tasks Component -->
    <center class="mt-5">
      <toDoList :tasks="tasks"/>
    </center>
    </div>
</template>

<script>
import toDoList from './toDoList.vue';

    export default {
        name:"addTask",
        components:{
            toDoList,
        },
        data(){
            return{
                task:"",
                tasks:[],
            }
        },
        methods:{
            // Method to Add Task to the toDo List Component
            addTask(e){
                e.preventDefault();
                this.tasks.push(this.task);
                // Add the data to localStorage
                var getTasks = JSON.parse(localStorage.getItem('tasks'));
                if(getTasks){
                    getTasks.push(this.task);
                    localStorage.setItem('tasks', JSON.stringify(getTasks));
                }
                else{
                    localStorage.setItem('tasks', JSON.stringify([this.task]));
                }
                this.task="";
            }
        },
        created(){
            // Get the data from localStorage
            localStorage.getItem('tasks') ? this.tasks = JSON.parse(localStorage.getItem('tasks')) : this.tasks = [];
        }
    }
</script>

<style scoped>

</style>