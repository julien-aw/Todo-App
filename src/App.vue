<template>
  <div class="w-full h-screen bg-gray-100 pt-8">
    <div class="bg-white p-3 max-w-md mx-auto">
      <div class="text-center">
        <h1 class="text-3xl font-bold">My Todo App</h1>
        <div class="mt-4 flex">
          <input
            class="w-80 border-b-2 border-gray-500 text-black"
            type="text"
            placeholder="Enter your task here"
            v-model="taskInput"
          />
          <button
            class="ml-2 border-2 border-green-500 p-2 text-green-500 hover:text-white hover:bg-green-500 rounded-lg flex"
            @click="addTask"
          >
            <svg
              class="h-6 w-6 text-green-600"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M12 9v3m0 0v3m0-3h3m-3 0H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
          </button>
        </div>
      </div>
      <div class="mt-8">
        <ul>
          <li
            class="p-2 rounded-lg"
            v-for="(task, index) in this.tasks"
            :key="index"
          >
            <div class="flex align-middle flex-row justify-between">
              <div class="p-2">
                <input
                  type="checkbox"
                  class="h-6 w-6"
                  :value="index"
                  v-model="completed"
                />
              </div>
              <div class="p-2" v-on:dblclick="updateTask(index)">
                <p class="text-lg text-gray-400 break-all">
                  {{ task.text }}
                </p>
                <p class="text-xs text-gray-400">
                  {{ task.date }}
                </p>
              </div>
              <button
                class="flex text-red-500 border-2 border-red-500 max-h-10 max-w-none p-2 rounded-lg hover:text-white hover:bg-red-500"
                @click="deleteTask(index)"
              >
                <svg
                  class="h-6 w-6"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                  stroke-width="2"
                  stroke="currentColor"
                  fill="none"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <path stroke="none" d="M0 0h24v24H0z" />
                  <line x1="4" y1="7" x2="20" y2="7" />
                  <line x1="10" y1="11" x2="10" y2="17" />
                  <line x1="14" y1="11" x2="14" y2="17" />
                  <path d="M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2 -2l1 -12" />
                  <path d="M9 7v-3a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v3" />
                </svg>
              </button>
            </div>
            <hr class="mt-2" />
            <div
              class="relative z-10"
              aria-labelledby="modal-title"
              role="dialog"
              aria-modal="true"
              v-if="viewModal"
            >
              <div
                class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
              ></div>

              <div class="fixed z-10 inset-0 overflow-y-auto">
                <div
                  class="flex items-end sm:items-center justify-center min-h-full p-4 text-center sm:p-0"
                >
                  <div
                    class="relative bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:max-w-lg sm:w-full"
                  >
                    <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                      <div class="sm:flex sm:items-start">
                        <div
                          class="mx-auto flex-shrink-0 flex items-center justify-center h-12 w-12 rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10"
                        >
                          <svg
                            class="h-6 w-6 text-green-600"
                            xmlns="http://www.w3.org/2000/svg"
                            fill="none"
                            viewBox="0 0 24 24"
                            stroke-width="2"
                            stroke="currentColor"
                            aria-hidden="true"
                          >
                            <path
                              stroke-linecap="round"
                              stroke-linejoin="round"
                              d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"
                            />
                          </svg>
                        </div>
                        <div
                          class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left"
                        >
                          <h3
                            class="text-lg leading-6 font-medium text-gray-900"
                            id="modal-title"
                          >
                            Update Task
                          </h3>
                          <div class="mt-2">
                            <input
                              class="w-80 border-b-2 border-gray-500 text-black"
                              type="text"
                              :placeholder="task.text"
                              v-model="newUpdate"
                            />
                          </div>
                        </div>
                      </div>
                    </div>
                    <div
                      class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse"
                    >
                      <button
                        type="button"
                        class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-green-600 text-base font-medium text-white hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500 sm:ml-3 sm:w-auto sm:text-sm"
                        @click="updateEnd()"
                      >
                        Update
                      </button>
                      <button
                        type="button"
                        class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-white-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm"
                        @click="canceled()"
                      >
                        Cancel
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </li>
        </ul>
      </div>
      <div class="mt-8">
        <button
          class="border-2 border-red-500 p-2 text-red-500"
          @click="clearCompleted()"
        >
          Clear Completed Task
        </button>
        <button
          class="border-2 border-indigo-500 p-2 text-indigo-500 ml-4"
          @click="reset()"
        >
          Reset Todo List
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      newUpdate: "",
      viewModal: false,
      tasks: [],
      completed: [],
      taskInput: "",
    };
  },
  methods: {
    addTask() {
      this.tasks.push({
        text: this.taskInput,
        date: Date().toString("YYYY-MM-dd").slice(0, 24),
      });
      this.taskInput = "";
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    updateTask() {
      this.viewModal = true;
    },
    updateEnd(newUpdate, index) {
      this.tasks.splice(index, 1, {
        text: this.newUpdate,
        date: Date().toString("YYYY-MM-dd").slice(0, 24),
      });
      this.newUpdate = "";
      this.viewModal = false;
    },
    canceled() {
      this.viewModal = false;
    },
    reset() {
      this.tasks = [];
    },
    clearCompleted() {
      for (var i = 0; i <= this.tasks.length; i++) {
        this.tasks.pop();
      }

      this.completed = [];
    },
  },
};
</script>
