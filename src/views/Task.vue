<template>
  <div id="task">
    <div id="card">
      <h3>{{ getDay }}</h3>
      <h3 style="color: #f86457;">{{ getCurrentDate }}</h3>
      <add-task @added-task="addTask"></add-task>
      <p v-if="isLoading">Loading...</p>
      <p v-if="tasks.length == 0" style="text-align:center;">Empty</p>
      <draggable
        v-model="tasks"
        group="people"
        @start="drag = true"
        @end="drag = false"
        v-else
      >
        <div v-for="task in tasks" :key="task.id">
          <item-task
            :task="task"
            @edited-task="editTask"
            @delete-task="deleteTask"
            @mark-done="markDone"
          >
          </item-task>
        </div>
      </draggable>
    </div>
  </div>
</template>

<script>
import AddTask from "../components/AddTask.vue";
import ItemTask from "../components/ItemTask";
import axios from "axios";
import draggable from "vuedraggable";

const baseUrl = "http://localhost:3000/todos";
var itemId = 0;

export default {
  name: "task",
  components: {
    AddTask,
    ItemTask,
    draggable
  },
  data() {
    return {
      tasks: [],
      isLoading: false
    };
  },
  created() {
    this.getTasks();
  },
  computed: {
    getCurrentDate() {
      var today = new Date();
      var dd = today.getDate();
      var mm = today.getMonth() + 1;
      var yyyy = today.getFullYear();
      if (dd < 10) {
        dd = "0" + dd;
      }
      if (mm < 10) {
        mm = "0" + mm;
      }
      today = mm + "-" + dd + "-" + yyyy;
      return today;
    },
    getDay() {
      var today = new Date();
      var weeks = [
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday"
      ];
      return weeks[today.getDay() - 1];
    }
  },
  methods: {
    getTasks() {
      axios.get(baseUrl).then(response => {
        this.tasks = response.data;
      });
    },
    addTask(description) {
      itemId++;
      this.isLoading = true;
      axios
        .post(baseUrl, {
          id: itemId,
          description: description,
          isdone: false
        })
        .then(response => {
          this.tasks = [...this.tasks, response.data];
          this.isLoading = false;
        });
    },
    deleteTask(id) {
      axios.delete(baseUrl + "/" + id).then(() => {
        this.getTasks();
      });
    },
    editTask(task) {
      axios
        .put(baseUrl + "/" + task.id, task)
        .then(() => {
          this.getTasks();
        });
    },
    markDone(task) {
      axios
        .put(baseUrl + "/" + task.id, task)
        .then(() => {
          this.getTasks();
        });
    }
  }
};
</script>

<style scope>
@import url("https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600&display=swap");

div #card {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 1rem auto;
  border-radius: 10px;
  padding: 2rem;
  text-align: left;
  background-color: white;
  width: 100%;
  max-width: 30rem;
}
</style>
