<template>
  <div class="xl:mx-56 2xl:mx-96 min-h-screen bg-gray-100">
    <div class="header">
      <Header />
    </div>
    <div class="body md:p-12 px-2 py-4">
      <div class="add-task">
        <AddTask />
      </div>
      <div class="todo-list">
        <TodoList />
      </div>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header"
import AddTask from "./components/AddTask"
import TodoList from "./components/TodoList"

export default {
  name: "App",
  components: {
    Header,
    AddTask,
    TodoList
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  height: 100%;
}

:focus {
  outline: 1px solid seagreen;
}

:disabled {
  opacity: 0.5;
}
</style>
