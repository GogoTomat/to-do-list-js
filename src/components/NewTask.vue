<template>
    <div class="new-task">
      <div class="nav">
        <button type="button" class="back-btn" @click="$emit('cancel')">
          <img src="../assets/leftstrelka.svg" alt="Back">
        </button>
        <button type="button" class="accept-btn" @click="addTask">
          <img src="../assets/gal.svg" alt="Accept">
        </button>
      </div>
      <textarea v-model="taskText" placeholder="Add new task..." class="textarea"></textarea>
    </div>
  </template>

  <script lang="ts">
  import { defineComponent, ref } from 'vue';

  export default defineComponent({
    name: 'NewTask',
    emits: ['add-task', 'cancel'],
    setup(props, { emit }) {
      const taskText = ref('');

      const addTask = () => {
        if (taskText.value.trim()) {
          emit('add-task', taskText.value.trim());
          taskText.value = '';
        }
      };

      return {
        taskText,
        addTask,
      };
    },
  });
  </script>

  <style scoped>
  .new-task {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: #E5E5E5;
  }

  .nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px;
  }

  .back-btn, .accept-btn {
    background-color: #E5E5E5;
    border: none;
  }

  .textarea {
    width: calc(100% - 40px);
    margin: 20px;
    border: none;
    background-color: #E5E5E5;
    font-size: 18pt;
    resize: none;
    outline: none;
  }
  </style>
