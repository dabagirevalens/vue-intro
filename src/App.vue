<template>
  <div class="container">
    <Header
      @toggle-add-task="toggleAddTask"
      :showAddTask="showAddTask"
      title="Task Tracker"
    />
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
  </div>
</template>

<script>
import Header from "./components/Header";
import Tasks from "./components/Tasks";
import AddTask from "./components/AddTask";

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
    };
  },

  methods: {
    async addTask(task) {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });

      const data = await res.json();

      this.tasks = [...this.tasks, data];
    },

    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },

    async deleteTask(id) {
      if (confirm("Are u sure ?")) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });

        res.status === 2000
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("Error while deleting task");
      }
    },

    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const upTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PATCH",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(upTask),
      });

      const data = res.json();

      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },

    async fetchTasks() {
      const res = await fetch("api/tasks");

      const data = await res.json();

      return data;
    },

    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);

      const data = await res.json();

      return data;
    },
  },

  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>

<style>
*,
*::before,
*::after {
  box-sizing: border-box;
}
* {
  margin: 0;
  font-family: "Satoshi", sans-serif;
}
html,
body {
  height: 100%;
}
body {
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
}
img,
picture,
video,
canvas,
svg {
  display: block;
  max-width: 100%;
}
input,
button,
textarea,
select {
  font: inherit;
}
p,
h1,
h2,
h3,
h4,
h5,
h6 {
  overflow-wrap: break-word;
}
#root,
#__next {
  isolation: isolate;
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
