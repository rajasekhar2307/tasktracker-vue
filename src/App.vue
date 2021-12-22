<template>
  <div class="container">
    <Header
      @toggle-add-task="toggleAddTask"
      title="Task Tracker"
      :showAddTask="this.showAddTask"
    />
    <div v-show="showAddTask">
      <AddTask
        @add-task="addTask"
        :editdata="editData"
        @edit-task-db="editTaskInDB"
      />
    </div>
    <Tasks
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      @edit-task="editTask"
      :tasks="tasks"
    />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Tasks from "./components/Tasks.vue";
import AddTask from "./components/AddTask.vue";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
      showAddTask: false,
      editData: {
        text: "",
        day: "",
        reminder: false,
      },
    };
  },
  methods: {
    async toggleReminder(id) {
      const getTask = await this.fetchTask(id);

      const toggledTask = { ...getTask, reminder: !getTask.reminder };

      const response = await fetch(`http://localhost:3000/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(toggledTask),
      });

      const data = await response.json();

      this.tasks = this.tasks.map((task) =>
        task.id == id ? { ...task, reminder: data.reminder } : task
      );
    },
    async addTask(newTask) {
      const response = await fetch("http://localhost:3000/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(newTask),
      });
      const data = await response.json();
      this.tasks.push(data);
    },
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    async fetchPosts() {
      const response = await fetch("http://localhost:3000/tasks");
      const data = await response.json();
      return data;
    },
    async fetchTask(id) {
      const response = await fetch(`http://localhost:3000/tasks/${id}`);
      const data = await response.json();
      return data;
    },
    async deleteTask(id) {
      const response = await fetch(`http://localhost:3000/tasks/${id}`, {
        method: "DELETE",
      });
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
    async editTask(id) {
      const data = await this.fetchTask(id);

      this.showAddTask = true;
      console.log(data);

      this.editData = { ...data };

      console.log(this.editData);
    },
    async editTaskInDB(data) {
      console.log("About to edit", data);

      this.deleteTask(data.id);
      delete data.id;
      this.addTask(data);
      // const response = await fetch(`http://localhost:3000/tasks/${id}`, {
      //   method: "PUT",
      //   headers: {
      //     "Content-type": "application/json",
      //   },
      //   body: JSON.stringify(data),
      // });

      // const retdata = await response.json();

      // console.log(retdata);
    },
  },

  async created() {
    this.tasks = await this.fetchPosts();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
