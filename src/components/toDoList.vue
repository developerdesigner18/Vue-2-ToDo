<template>
    <div class="grid grid-cols-1 w-3/5 mx-auto place-items-center gap-y-4 lg:grid-cols-2 lg:gap-x-4">

        <div class="w-full max-w-sm p-4 bg-white border border-gray-200 rounded-lg shadow sm:p-6 dark:bg-gray-800 dark:border-gray-700 min-h-[450px]">
            <!-- To Do Task List Title -->
            <h5 class="mb-3 text-base font-semibold text-gray-900 md:text-xl dark:text-white">
                To Do Task List 
            </h5>
           <!-- To Do Task List Items -->
            <ul class="my-4 space-y-3">
                <li v-for="task in localTasks" :key="task">
                    <p class="flex items-center justify-between p-3 text-base font-bold text-gray-900 rounded-lg bg-gray-50 hover:bg-gray-100 group hover:shadow dark:bg-gray-600 dark:hover:bg-gray-500 dark:text-white">
                        <span class="whitespace-nowrap">{{ task }}</span>
                        <span>
                            <!-- Completed Task Icon -->
                            <i @click="taskCompleted(task)" class="fa-solid fa-square-check mr-4" style="color: #41da3e;"></i>
                            <!-- Delete Task Icon -->
                            <i @click="deleteTask(task)" class="fa-solid fa-trash" style="color: #e81717;"></i>
                        </span>
                    </p>
                </li>
            </ul>
            
        </div>
        <!-- Completed Task List Component -->
        <completedTask :completedTasks="completedTasks"></completedTask>
    </div>
</template>

<script>
import completedTask from './completedTask.vue';
    export default {
        name: "toDoList",
        components:{
            completedTask
        },
        props:{
            tasks:Array   
        },
        data(){
            return{
                completedTasks:[],
                localTasks:this.tasks
            }
        },
        methods:{
            // Method to mark the task as completed and send it to the completedTasks Component
            taskCompleted(task){
                 // Find the index of the completed task in the localTasks array
                const index = this.localTasks.indexOf(task);
                if (index !== -1) {
                    // Remove the task from the localTasks array
                    this.localTasks.splice(index, 1);
                }
                // Push the completed task to the completedTasks array
                this.completedTasks.push(task);
                // Add the data to localStorage
                var getCompletedTasks = JSON.parse(localStorage.getItem('completedTasks'));
                if(getCompletedTasks){
                    getCompletedTasks.push(task);
                    localStorage.setItem('completedTasks', JSON.stringify(getCompletedTasks));
                    localStorage.setItem('tasks', JSON.stringify(this.localTasks));
                }
                else{
                    localStorage.setItem('completedTasks', JSON.stringify([task]));
                    localStorage.setItem('tasks', JSON.stringify(this.localTasks));
                }
                
            },
            // Method to delete the task
            deleteTask(task){
                // Find the index of the completed task in the localTasks array
                const index = this.localTasks.indexOf(task);
                if (index !== -1) {
                    // Remove the task from the localTasks array
                    this.localTasks.splice(index, 1);
                }
                // Remove the data from localStorage
                localStorage.setItem('tasks', JSON.stringify(this.localTasks));
            }
        },
        created(){
            // Get the data from localStorage
            localStorage.getItem('completedTasks') ? this.completedTasks = JSON.parse(localStorage.getItem('completedTasks')) : this.completedTasks = [];
        }
    }
</script>

<style scoped>

</style>