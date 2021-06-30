<template>
  <c-flex
    v-bind="mainStyles[colorMode]"
    align="start"
    justify="start"
    direction="column"
    height="100vh"
    p="1rem"
  >
    <c-flex
      role="navigation"
      p="1rem"
      justify="space-between"
      align="center"
      width="100%"
    >
      <h4>Kanban</h4>
      <c-stack is-inline align="center" gap="10px">
        <a
          href="https://github.com/shubhamrangaa/kanban-board-vue"
          target="_blank"
          >GitHub</a
        >
        <!-- <c-switch color="cyan" size="lg" @click="$toggleColorMode" /> -->

        <CIconButton
          cursor="pointer"
          border="none"
          :icon="colorMode === 'light' ? 'moon' : 'sun'"
          :variant-color="colorMode === 'light' ? 'indigo' : 'yellow'"
          @click="$toggleColorMode"
          :aria-label="
            `Switch to ${colorMode === 'light' ? 'dark' : 'light'} mode`
          "
        />
      </c-stack>
    </c-flex>

    <c-divider />
    <c-box m="0 auto" maxW="1250px">
      <c-heading as="h1" size="2xl" mb="2rem">
        ⚡ Kanban Board
      </c-heading>

      <board></board>
    </c-box>
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
