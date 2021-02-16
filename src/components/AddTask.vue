<template>
  <div class="mx-auto">
    <form class="flex" @submit.prevent="callAddTaskEvent()">
      <input
        type="text"
        name="task-input"
        id="task-input"
        placeholder="Start writing your new task"
        class="w-full h-10 pl-3 rounded"
        v-model="task"
        required
        minlength="5"
        maxlength="150"
      />
      <button type="submit" class="bg-blue-500 text-white w-36 font-bold rounded-r">ADD</button>
    </form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      task: ''
    }
  },
  methods: {
    callAddTaskEvent() {
      this.$root.$emit('addTaskEvent', this.task)
      this.task = ''
    }
  }
}
</script>

<style></style>
