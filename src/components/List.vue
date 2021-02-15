<template>
  <v-container fill-height fluid>
    <v-row align="center" justify="center">
      <v-col>
        <v-card class="mx-auto" max-width="600" tile elevation="12">
          <v-overlay :absolute="absolute" :opacity="opacity" :value="overlay">
            <v-col class="blue-grey lighten-3">
              <v-text-field
                label="Add task"
                v-model="currentTask"
              ></v-text-field>
              <v-btn color="orange lighten-2" @click="updateTask(currentTask)">
                Update task
              </v-btn>
            </v-col>
          </v-overlay>

          <v-toolbar color="light-blue" height="70px" dark>
            <v-toolbar-title>My To Do List</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-row class="shrink mx-1" align="center" justify="center">
              <v-btn icon @click="expandInput = !expandInput">
                <v-icon>mdi-magnify</v-icon>
              </v-btn>
              <v-expand-x-transition>
                <v-text-field
                  v-show="expandInput"
                  v-model="searchValue"
                  v-on:keyup="filterValue"
                  placeholder="Search task"
                ></v-text-field>
              </v-expand-x-transition>
            </v-row>
          </v-toolbar>
          <v-list-item>
            <v-text-field
              label="Add task"
              v-model="eneteredValue"
              v-on:keyup.enter="addTask"
            ></v-text-field>
          </v-list-item>
          <v-slide-y-transition class="py-0" group>
            <v-list-item
              v-for="(task, index) in filteredTasks"
              v-bind:key="index"
            >
              <v-btn icon @click="editTask(task, index)">
                <v-icon color="grey lighten-1">mdi-pencil</v-icon>
              </v-btn>
              <v-list-item-content>
                <v-list-item-title>{{ task }}</v-list-item-title>
              </v-list-item-content>
              <v-list-item-action>
                <v-btn icon @click="deleteTask(index)">
                  <v-icon color="grey lighten-1">mdi-delete</v-icon>
                </v-btn>
              </v-list-item-action>
            </v-list-item>
          </v-slide-y-transition>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "List",

  data() {
    return {
      tasks: [],
      filteredTasks: [],
      eneteredValue: "",
      searchValue: "",
      expandInput: false,
      absolute: true,
      opacity: 0.6,
      overlay: false,
      currentTask: "",
      currentIndex: "",
    };
  },

  methods: {
    addTask() {
      if (this.eneteredValue.length !== 0) {
        if (!this.tasks.some((task) => task === this.eneteredValue)) {
          this.tasks.push(this.eneteredValue);
          this.eneteredValue = "";
        }
      }
      this.filteredTasks = [...this.tasks];
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      this.filteredTasks.splice(index, 1);
    },
    editTask(task, index) {
      this.overlay = !this.overlay;
      this.currentTask = task;
      this.currentIndex = index;
    },
    updateTask() {
      this.overlay = false;
      this.tasks[this.currentIndex] = this.currentTask;
      this.filteredTasks = [...this.tasks];
    },
    filterValue() {
      let intermedArr = [];
      if (this.searchValue.length === 0) {
        this.filteredTasks = [...this.tasks];
      } else {
        for (const task of this.tasks) {
          if (task.includes(this.searchValue)) {
            intermedArr.push(task);
          }
        }
        this.filteredTasks = [...intermedArr];
      }
    },
  },
};
</script>
