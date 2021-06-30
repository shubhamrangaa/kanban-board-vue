<template>
  <c-flex
    v-bind="mainStyles[colorMode]"
    align="start"
    justify="start"
    direction="column"
    height="100vh"
  >
    <nav role="navigation" aria-label="main navigation">
      <h4>Kanban</h4>
      <a clas href="https://github.com/shubhamrangaa/kanban-board-vue"
        >View code on GitHub</a
      >
    </nav>

    <c-divider />
    <c-stack is-inline>
      <c-switch color="cyan" size="lg" @click="$toggleColorMode" />
    </c-stack>

    Chakra ColorMode: {{ colorMode }}
    <h1>Kanban Board</h1>
    <board></board>
  </c-flex>
</template>

<script>
import Board from "../components/Board.vue";

export default {
  name: "HomePage",
  inject: ["$chakraColorMode", "$toggleColorMode"],
  components: { Board },
  data() {
    return {
      mainStyles: {
        dark: {
          bg: "gray.700",
          color: "whiteAlpha.900"
        },
        light: {
          bg: "white",
          color: "gray.900"
        }
      }
    };
  },
  computed: {
    theme() {
      return this.$chakraTheme();
    },
    colorMode() {
      return this.$chakraColorMode();
    }
  }
};
</script>

<style>
@import "../assets/styles/board.scss";
</style>
