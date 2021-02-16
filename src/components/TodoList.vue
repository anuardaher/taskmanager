<template>
  <div>
    <div class="todo mt-6 mb-12">
      <h1 class="title text-2x1 text-green-700 font-extrabold">TO DO</h1>
      <table class="w-full my-4 table-fixed table-height" v-if="todoList.length > 0">
        <transition-group name="table" tag="tbody">
          <tr
            v-for="(task, index) in todoList"
            :key="task"
            class="h-10 border-b-2 table-task fade"
          >
            <td class="w-10">
              <input
                type="checkbox"
                name="task"
                class="h-5 w-5"
                @click="(event) => selectTask(task, index, event)"
              />
            </td>
            <td class="w-full text-gray-400">
              <span v-text="task" />
            </td>
            <td class="w-10 text-center">
              <button @click="editTask(index)">
                <icon class="text-gray-500 h-6 w-6" name="pencil" />
              </button>
            </td>
            <td class="w-10 text-center">
              <button @click="showDeleteConfirmation('todoList', index)">
                <icon class="text-gray-500 h-6 w-6" name="delete" />
              </button>
            </td>
          </tr>
        </transition-group>
      </table>
      <div v-else class="mt-4 text-gray-500">
        Created tasks will be shown here 🎈
      </div>
    </div>
    <div class="done">
      <h1 class="title text-2x1 text-green-700 font-extrabold">DONE</h1>
      <table class="w-full my-6 table-fixed" v-if="doneList.length > 0">
        <transition-group name="table" tag="tbody">
          <tr
            v-for="(task, index) in doneList"
            :key="task"
            class="h-8 border-b-2 table-task"
          >
            <td class="w-10">
              <input
                type="checkbox"
                name="task"
                class="h-5 w-5"
                checked
                @click="(event) => selectTask(task, index, event)"
              />
            </td>
            <td class="w-full text-gray-400">
              <span class=""
                ><del>{{ task }}</del></span
              >
            </td>
            <td class="w-10 text-center">
              <button @click="showDeleteConfirmation('doneList', index)">
                <icon name="delete" class="text-gray-500 h-6 w-6" />
              </button>
            </td>
          </tr>
        </transition-group>
      </table>
      <div v-else class="mt-4 text-gray-500">
        Nothing done at this time 🥱
      </div>
    </div>
    <!-- Modal de edição de tarefa -->
    <modal
      v-if="showEditDialog"
      title="Editar Tarefa"
      type="success"
      @close="showEditDialog = false"
    >
      <template v-slot:body>
        <textarea
          required
          name="task"
          maxlength="150"
          class="w-full bg-white resize-none p-2 border-2"
          rows="5"
          ref="edit-textarea"
          v-model="editedTask.description"
        />
      </template>
      <template v-slot:footer>
        <button
          :disabled="
            editedTask.description.length < 5 ||
              editedTask.description.length > 150
          "
          type="button"
          class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-green-600 text-base font-medium text-white hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500 sm:ml-3 sm:w-auto sm:text-sm"
          @click="saveTask()"
        >
          Salvar
        </button>
      </template>
    </modal>
    <!-- Modal de confirmação de exclusão de tarefa -->
    <modal
      v-if="showDeleteConfirmationDialog"
      title="Excluir Tarefa"
      type="error"
      icon-name="delete"
      @close="showDeleteConfirmationDialog = false"
    >
      <template v-slot:body>
        <p>Are you sure you want to delete this task?</p>
      </template>
      <template v-slot:footer>
        <button
          type="button"
          class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-red-600 text-base font-medium text-white hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500 sm:ml-3 sm:w-auto sm:text-sm"
          @click="deleteTask()"
        >
          Sim
        </button>
      </template>
    </modal>
  </div>
</template>

<script>
import Icon from "./AppIcon"
import Modal from "./Modal"

export default {
  components: {
    Icon,
    Modal
  },
  data() {
    return {
      todoList: [],
      doneList: [],
      showEditDialog: false,
      showDeleteConfirmationDialog: false,
      editedTask: {
        index: null,
        description: ""
      },
      deletedTask: {
        index: null,
        list: null
      }
    }
  },
  methods: {
    selectTask(task, index, event) {
      if (event.target.checked) {
        this.todoList.splice(index, 1)
        this.doneList.unshift(task)
      } else {
        this.doneList.splice(index, 1)
        this.todoList.unshift(task)
      }
      this.setLocalStorage()
    },
    editTask(index) {
      this.showEditDialog = true
      this.editedTask.description = this.todoList[index]
      this.editedTask.index = index
      setTimeout(() => this.$refs["edit-textarea"].focus(), 100)
    },
    saveTask() {
      this.todoList[this.editedTask.index] = this.editedTask.description
      this.showEditDialog = false
      this.setLocalStorage()
    },
    showDeleteConfirmation(list, index) {
      this.deletedTask.index = index
      this.deletedTask.list = list
      this.showDeleteConfirmationDialog = true
    },
    deleteTask() {
      let { list, index } = this.deletedTask
      this[list].splice(index, 1)
      this.showDeleteConfirmationDialog = false
      this.setLocalStorage()
    },
    getLocalStorage() {
      this.todoList = JSON.parse(localStorage.getItem("todo")) || []
      this.doneList = JSON.parse(localStorage.getItem("done")) || []
    },
    setLocalStorage() {
      localStorage.setItem("todo", JSON.stringify(this.todoList))
      localStorage.setItem("done", JSON.stringify(this.doneList))
    }
  },
  mounted() {
    this.getLocalStorage()
    this.$root.$on("addTaskEvent", (task) => {
      if (task) {
        this.todoList.unshift(task)
        localStorage.setItem("todo", JSON.stringify(this.todoList))
      }
    })
  }
}
</script>

<style>
.table-task {
  transition: all 0.5s;
}

</style>
