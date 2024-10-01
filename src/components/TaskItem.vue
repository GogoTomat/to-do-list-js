<template>
    <div class="todo-item">
      <label class="checkbox-container">
        <input type="checkbox" class="done_checked" v-model="task.completed" @change="toggleTask">
        <span class="checkmark"></span>
      </label>
      <p :class="['task-text', { 'completed': task.completed }]">{{ task.text }}</p>
      <button type="button" class="button_delete" @click="deleteTask">
        <img src="../assets/vue.svg" alt="Delete task">
      </button>
    </div>
  </template>

  <script lang="ts">
  import { defineComponent } from 'vue';

  export default defineComponent({
    name: 'TaskItem',
    props: {
      task: {
        type: Object,
        required: true,
      },
    },
    emits: ['delete-task', 'toggle-task'],
    methods: {
      deleteTask() {
        this.$emit('delete-task', this.task.id);
      },
      toggleTask() {
        this.$emit('toggle-task', this.task.id);
      },
    },
  });
  </script>

  <style scoped>
  .todo-item {
    background-color: #FFFFFF;
    display: flex;
    align-items: center;
    border-radius: 10px;
    position: relative;
    margin-bottom: 10px;
  }

  .checkbox-container {
    display: inline-block;
    position: relative;
    margin: 15px;
    width: 22px;
    height: 22px;
  }

  .done_checked {
    display: none;
  }

  .checkmark {
    width: 22px;
    height: 22px;
    border-radius: 50%;
    background-color: #E5E5E5;
    display: inline-block;
    transition: background-color 0.3s;
  }

  .checkbox-container input:checked + .checkmark {
    background-color: #37D7B3;
    background-image: url('@/assets/galka.svg');
    background-size: 11px 9px;
    background-repeat: no-repeat;
    background-position: center;
  }

  .task-text {
    margin-top: 15px;
    margin-bottom: 15px;
  }

  .task-text.completed {
    text-decoration: line-through;
  }

  .button_delete {
    position: absolute;
    right: 0;
    margin-right: 15px;
    background-color: #F80F0F;
    border-radius: 50%;
    width: 22px;
    height: 22px;
    padding: 0;
  }
  </style>
