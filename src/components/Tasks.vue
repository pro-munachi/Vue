<template>
  <div>
    <div v-if="tasks.length < 1">
      <p>No task at the moment</p>
    </div>
    <div v-if="tasks.length > 0">
      <div v-for="task in tasks" :key="task.id">
        <TaskComponent
          @toggle-reminder="$emit('toggle-reminder', task.id)"
          @delete-task="$emit('delete-task', task.id)"
          :task="task"
        />
      </div>
    </div>
  </div>
</template>

<script>
import TaskComponent from "./Task.vue";

export default {
  name: "TasksComponent",

  props: {
    tasks: Array,
  },

  components: {
    TaskComponent,
  },

  emits: ["delete-task", "toggle-reminder"],
};
</script>

<style scoped></style>
