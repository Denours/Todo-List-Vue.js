<template>
  <Layout>
    <template #header> En-tête </template>
    <template #aside> Sidebar </template>
    <template #main> Main </template>
    <template #footer> Footer </template>
  </Layout>
  <Button><b>Voyage</b></Button>
  <button @click="showTimer = !showTimer">Afficher / Masquer</button>
  <Timer v-if="showTimer" />
  <div class="container">
    <h1>Todo-List</h1>
    <p v-if="tasks.length === 0">Aucune tâche à faire pour le moment !😄</p>
    <form action="" @submit.prevent="addTask">
      <input
        type="text"
        placeholder="Tâche à effectuer"
        v-model="newTaskTitle"
      />
      <button :disabled="newTaskTitle.length === 0" class="bg-pink-600">
        Ajouter
      </button>
    </form>
    <ul>
      <li
        :key="task.date"
        v-for="task in sortedTasks"
        :class="{ active: task.completed }"
      >
        <Checkbox :label="task.title" v-model="task.completed" />
      </li>
    </ul>
    <label>
      <input type="checkbox" v-model="hideCompleted" />
      Masquer les tâches complétées
    </label>
    <p v-if="remainingTasks > 0">
      {{ remainingTasks }} tâche{{ remainingTasks > 1 ? "s" : "" }} restantes à
      effectuer
    </p>
    <!-- <Checkbox :label="'Youpii !'" /> -->
  </div>
</template>

<script setup>
import { computed, onMounted, ref } from "vue";
import Checkbox from "../Checkbox.vue";
import Button from "../Button.vue";
import Layout from "../Layout.vue";
import Timer from "./Timer.vue";
const tasks = ref([]);
const showTimer = ref(true);
onMounted(() => {
  fetch("https://jsonplaceholder.typicode.com/todos")
    .then((r) => r.json())
    .then((v) => (tasks.value = v.map((task) => ({ ...task, date: task.id }))));
});
const newTaskTitle = ref("");
const hideCompleted = ref(false);
const addTask = () => {
  tasks.value.push({
    title: newTaskTitle.value,
    completed: false,
    date: Date.now(),
  });
  newTaskTitle.value = "";
};
const sortedTasks = computed(() => {
  const sortedTodos = tasks.value.toSorted((a, b) =>
    a.completed > b.completed ? 1 : -1,
  );
  if (hideCompleted.value === true) {
    return sortedTodos.filter((t) => t.completed === false);
  }
  return sortedTodos;
});

const remainingTasks = computed(() => {
  return sortedTasks.value.filter((t) => t.completed === false).length;
});
</script>

<style>
.active {
  color: red;
  text-decoration: line-through;
}

.container {
  padding: 50px;
  border: 2px solid black;
  border-radius: 10px;
  width: 20%;
  margin: 100px;
  background-color: hsl(120, 51%, 85%);
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
form input {
  border: 2px solid hsl(0, 6%, 58%);
  border-radius: 5px;
}
form button {
  margin-left: 10px;
}
</style>
