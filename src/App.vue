<template>
  <h1>Kanban Vue project</h1>
  <div id="app" style="height: 25rem;">
    <KanbanBoard />
  </div>
</template>

<script>
import KanbanBoard from "@/components/KanbanBoard.vue";

export default {
  components: {
    KanbanBoard,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>