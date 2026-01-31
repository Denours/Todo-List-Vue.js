<template>
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
    <Checkbox :label="'Yas !'" />
  </div>
</template>

<script setup>
import { computed, ref } from "vue";
import Checkbox from "../Checkbox.vue";
const tasks = ref([
  {
    title: "Promener mon chien",
    completed: false,
    date: 1,
  },
  {
    title: "Manger une pizza",
    completed: false,
    date: 2,
  },
  {
    title: "Laver le sol",
    completed: false,
    date: 3,
  },
  {
    title: "Aller au manège",
    completed: false,
    date: 4,
  },
  {
    title: "Apprendre Vue.js",
    completed: false,
    date: 5,
  },
]);
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
