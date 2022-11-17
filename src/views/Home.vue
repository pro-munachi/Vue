<template>
  <div class="container">
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>

    <router-view></router-view>
    <TasksComponent
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
  </div>
</template>

<script>
import TasksComponent from "../components/Tasks";
import AddTask from "../components/AddTask";

export default {
  name: "HomePage",
  props: {
    showAddTask: Boolean,
  },
  components: {
    TasksComponent,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    };
  },

  methods: {
    async deleteTask(id) {
      if (confirm("Are you sure you want to delete?")) {
        const res = await fetch(`http://localhost:5000/tasks/${id}`, {
          method: "DELETE",
          headers: {
            "Content-Type": "application/json",
          },
        });

        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("Error while deleting");
      }
    },

    async toggleReminder(id) {
      if (confirm("Are you sure you want to toggle?")) {
        // this.tasks = this.tasks.filter((task) => task.id !== id);

        const taskToToggle = await this.fetchTask(id);

        const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

        const res = await fetch(`http://localhost:5000/tasks/${id}`, {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(updTask),
        });

        if (res.status === 200) {
          for (let i = 0; i < this.tasks.length; i++) {
            if (this.tasks[i].id === id) {
              this.tasks[i].reminder = !this.tasks[i].reminder;
            }
          }
        } else {
          alert("Unable to update task");
        }
      }
    },

    async addTask(data) {
      const res = await fetch("http://localhost:5000/tasks", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(data),
      });

      const dat = await res.json();

      this.tasks.push(dat);
    },

    async fetchTasks() {
      const res = await fetch("http://localhost:5000/tasks");

      const data = res.json();

      return data;
    },

    async fetchTask(id) {
      const res = await fetch(`http://localhost:5000/tasks/${id}`);

      const data = res.json();

      return data;
    },
  },

  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>
