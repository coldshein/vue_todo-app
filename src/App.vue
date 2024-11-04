<script setup>
import originalTodos from "./data/todos";
import { computed, ref } from "vue";

const todos = ref(originalTodos);
const title = ref('');

const addTodo = () => {
  if (!title.value) return;

  todos.value.push({
    id: Date.now(),
    title: title.value,
    completed: false,
  });
};

const activeTodos = computed(() => {
  return todos.value.filter((todo) => !todo.completed);
});

const status = ref('all');

const visibleTodos = computed(() => {
  if (status.value === 'active') {
    return activeTodos.value;
  } if (status.value === "completed") {
    return todos.value.filter((todo) => todo.completed);
  }
  return todos.value;
})

</script>

<template>
  <div class="todoapp">
    <h1 class="todoapp__title">todos {{ todos.length }}</h1>

    <div class="todoapp__content">
      <header class="todoapp__header">
        <button type="button" v-if="todos.length > 0" class="todoapp__toggle-all"
          :class="{ active: activeTodos.length === 0 }" />
        <form @submit.prevent="addTodo">
          <input data-cy="NewTodoField" type="text" class="todoapp__new-todo" placeholder="What needs to be done?"
            v-model="title" />
        </form>
      </header>

      <section class="todoapp__main">
        <div v-for="(todo, index) of visibleTodos" :key="todo.id" class="todo" :class="{ completed: todo.completed }">
          <label class="todo__status-label">
            <input type="checkbox" class="todo__status" v-model="todo.completed" />
          </label>

          <form v-if="false">
            <input class="todo__title-field" placeholder="Empty todo will be deleted" />
          </form>

          <template v-else>
            <span class="todo__title">{{ todo.title }}</span>
            <button class="todo__remove" @click="todos.splice(i, 1)">×</button>
          </template>

          <div class="modal overlay" :class="{ 'is-active': false }">
            <div class="modal-background has-background-white-ter"></div>
            <div class="loader"></div>
          </div>
        </div>
      </section>

      <footer class="todoapp__footer">
        <span class="todo-count"> {{ activeTodos.length }} items left </span>

        <nav class="filter">
          <a href="#/" class="filter__link" :class="{ selected: status === 'all' }" @click="status = 'all'">
            All
          </a>
          <a href="#/active" class="filter__link" :class="{ selected: status === 'active' }" @click="status = 'active'">
            Active
          </a>
          <a href="#/completed" class="filter__link" :class="{ selected: status === 'completed' }"
            @click="status = 'completed'">
            Completed
          </a>
        </nav>
        <button type="button" class="todoapp__clear-completed" :disabled="todos.length === activeTodos.length">
          Clear completed
        </button>
      </footer>
    </div>

    <div class="notification is-danger is-light has-text-weight-normal">
      <button type="button" class="delete" />
      Unable to load todos
      <br />
      Title should not be empty
      <br />
      Unable to add a todo
      <br />
      Unable to delete a todo
      <br />
      Unable to update a todo
    </div>
  </div>
</template>
