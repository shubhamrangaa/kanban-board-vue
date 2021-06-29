<template>
  <section class="homeContainer">
    <h1>Kanban Board</h1>
    <board></board>
  </section>
</template>

<script>
import Board from "../components/Board.vue";
export default {
  name: "HomePage",

  components: { Board }
};
</script>

<style>
@import "../assets/styles/board.scss";
</style>
