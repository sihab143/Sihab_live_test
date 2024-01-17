<script setup>
import { ref } from 'vue';
const tasks = ref([
    {  name: "Task 1",  time: 30 }, 
    {  name: "Task 2",  time: 40 },
    {  name: "Task 3",  time: 60 },
    {  name: "Task 4",  time: 45 },
    {  name: "Task 5",  time: 50 }
    ]);

const taskName = ref('');
const taskTime = ref('');
const isPopupVisible = ref(false);
const showError = ref(false);
const isEditing = ref(false);
const editingIndex = ref(-1);

function openEditModal(index) {
    const task = tasks.value[index];
    taskName.value = task.name;
    taskTime.value = task.time;
    isEditing.value = true;
    editingIndex.value = index;
    openPopup();
}

function errorShow() {
    showError.value = true;
}

function openPopup() {
    isPopupVisible.value = true;
}

function closePopup() {
    isPopupVisible.value = false;
    showError.value = false;
}

function submitTask() {
    if (taskName.value && taskTime.value) {
        const newTask = {
            name: `${taskName.value}`,
            time: `${taskTime.value}`,
        }

        if (isEditing.value) {
            tasks.value[editingIndex.value] = newTask;
        } else {
            tasks.value.push(newTask);
        }

        taskName.value = "";
        taskTime.value = "";
        isEditing.value = false;
        editingIndex.value = -1;
        closePopup();

    } else {
        errorShow();
    }

}

</script>

<template>
    <ol class="list-decimal">
        <li v-for="(task, index) in tasks" :key="index">
            <b>Name:</b> {{ task.name }}
            <b>Time:</b> {{ task.time }}
            <button class="mx-5 mt-2 bg-blue-400 text-white hover:bg-blue-500 hover:border-blue-500 focus:outline-none"
                @click="openEditModal(index)">Edit Task</button>
        </li>
    </ol>


    <!-- PopUp modal-->
    <div class="w-full max-w-xs popup" v-if="isPopupVisible">
        <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4" @submit.prevent="submitTask()">
            <div class="mb-4">
                <label class="block text-gray-700 text-l font-bold mb-2" for="task-name">
                    Task Name
                </label>
                <input
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="task-name" type="text" placeholder="Task Name" v-model="taskName">
            </div>
            <div class="mb-6">
                <label class="block text-gray-700 text-l font-bold mb-2" for="Time">
                    Time
                </label>
                <input
                    class="shadow appearance-none border  rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="time" type="number" placeholder="Time" v-model="taskTime">
            </div>
            <div v-if="showError">
                <p class="text-red-500 text-sm mb-2 italic">Please provide both name and time for the task.</p>
            </div>
            <div class="flex items-center justify-between">
                <button
                    class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
                    type="submit">
                    Update Task
                </button>
                <button
                    class="bg-red-400 hover:border-red-500 hover:bg-red-500 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
                    type="button" @click="closePopup()">
                    Cancel
                </button>
            </div>
        </form>
    </div>
</template>

<style scoped>
.popup {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 999;
}
</style>