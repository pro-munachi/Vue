<template>
  <header>
    <h1>{{ title }}</h1>

    <HeaderButton
      @btn-click="$emit('show-task')"
      :text="showAddTask ? 'Close' : 'Add Task'"
      :color="showAddTask ? 'red' : 'green'"
    />
  </header>
</template>

<script>
import HeaderButton from "./HeaderButton.vue";

export default {
  name: "MyHeader",

  props: {
    title: String,
    showAddTask: Boolean,
  },

  components: {
    HeaderButton,
  },

  emits: ["show-task"],
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
