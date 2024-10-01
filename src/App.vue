<template>
  <div id="app">
    <EmptyState v-if="tasks.length === 0 && !isAddingTask" />
    <div v-else-if="!isAddingTask">
      <div class="tasks">
        <TaskItem
          v-for="task in tasks"
          :key="task.id"
          :task="task"
          @delete-task="deleteTask"
          @toggle-task="toggleTask"
        />
      </div>
    </div>
    <NewTask v-if="isAddingTask" @add-task="addTask" @cancel="isAddingTask = false" />
    <button v-if="!isAddingTask" type="button" class="button-add" @click="isAddingTask = true">
      <img src="./assets/crest.svg" alt="Add task" />
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref, watch } from 'vue';
import EmptyState from './components/EmptyState.vue';
import TaskItem from './components/TaskItem.vue';
import NewTask from './components/NewTask.vue';

interface Task {
  id: number;
  text: string;
  completed: boolean;
}

export default defineComponent({
  name: 'App',
  components: {
    EmptyState,
    TaskItem,
    NewTask,
  },
  setup() {
    const tasks = reactive<Task[]>([]);
    const isAddingTask = ref(false);
    let nextId = 1;

    const storedTasks = localStorage.getItem('tasks');
    if (storedTasks) {
      try {
        const parsedTasks = JSON.parse(storedTasks) as Task[];
        tasks.push(...parsedTasks);

        nextId = parsedTasks.reduce((maxId, task) => Math.max(maxId, task.id), 0) + 1;
      } catch (e) {
        console.error('Ошибка при парсинге задач из localStorage', e);
        localStorage.removeItem('tasks');
      }
    }

     watch(
      tasks,
      (newTasks) => {
        localStorage.setItem('tasks', JSON.stringify(newTasks));
      },
      { deep: true }
    );

    const addTask = (text: string) => {
      tasks.push({ id: nextId++, text, completed: false });
      isAddingTask.value = false;
    };

    const deleteTask = (id: number) => {
      const index = tasks.findIndex((task) => task.id === id);
      if (index !== -1) {
        tasks.splice(index, 1);
      }
    };

    const toggleTask = (id: number) => {
      const task = tasks.find((task) => task.id === id);
      if (task) {
        task.completed = !task.completed;
      }
    };

    return {
      tasks,
      isAddingTask,
      addTask,
      deleteTask,
      toggleTask,
    };
  },
});
</script>

<style>
@import './style.css';

#app {
  position: relative;
  height: 100vh;
  background-color: #E5E5E5;
}

.tasks {
  position: absolute;
  top: 30px;
  left: 50%;
  transform: translateX(-50%);
  width: calc(100% - 100px);
}

.button-add {
  padding: 17px;
  border: none;
  background: #FFB309;
  width: 63px;
  height: 63px;
  border-radius: 50%;
  position: absolute;
  bottom: 50px;
  left: 50%;
  transform: translateX(-50%);
}
</style>
