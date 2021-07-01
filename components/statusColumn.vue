<template>
  <c-box m="10px" width="240px" minH="100px">
    <c-badge variant-color="indigo" mb="5px" font-size=".85em"
      >{{ name }} - {{ alteredList.length }}
    </c-badge>
    <draggable v-model="alteredList" v-bind="dragOptions" class="draggable">
      <transition-group>
        <c-box
          :backgroundColor="colorMode === 'light' ? '#fff' : '#202836'"
          :boxShadow="colorMode === 'light' ? '0px 3px 8px #e2e2e2' : '0'"
          cursor="grab"
          p="12px 10px"
          m="4px 0px"
          v-for="element in alteredList"
          :key="element.id"
          @click="$emit('open-modal', element, alteredList, name)"
        >
          {{ element.t_name }}
        </c-box>
      </transition-group>
    </draggable>
    <!-- ADD NEW TASK -->

    <c-input
      placeholder="+ New"
      v-model="newtask"
      class="inputNew"
      variant="unstyled"
      @keydown.native.enter="add()"
    ></c-input>
  </c-box>
</template>

<script>
import draggable from "vuedraggable";
import { v4 as uuidv4 } from "uuid";
export default {
  name: "StatusColumn",
  components: {
    draggable
  },
  inject: ["$chakraColorMode", "$toggleColorMode"],
  props: ["list", "name"],
  data() {
    return {
      alteredList: this.list,
      incomingList: "",
      newtask: "",
      isCardModalActive: false,
      editable: true,
      isDragging: false,
      delayedDragging: false
    };
  },
  methods: {
    add() {
      if (this.newtask) {
        console.log("using comp method");
        console.log(this.newtask);
        console.log(this.alteredList);
        this.alteredList.push({ t_name: this.newtask, id: uuidv4() });
        this.newtask = "";
      }
    }
  },
  computed: {
    colorMode() {
      console.log(this.$chakraColorMode());
      return this.$chakraColorMode();
    },
    dragOptions() {
      return {
        animation: 250,
        group: "tasks",
        disabled: !this.editable,
        ghostClass: "ghost"
      };
    },
    serial() {
      if (process.browser) {
        let existing = JSON.parse(localStorage.getItem(`taskList`) || "[]");
        var i = 0;
        while (i < 100) {
          if (existing[i].name == this.name) {
            break;
          }
          i++;
        }
      }
      return i;
    }
  },
  watch: {
    alteredList(newstuff) {
      if (process.browser) {
        let existing = JSON.parse(localStorage.getItem(`taskList`) || "[]");
        existing[this.serial].content = newstuff;
        localStorage.setItem("taskList", JSON.stringify(existing));
        console.log("watching component");
      }
    },
    isDragging(newValue) {
      if (newValue) {
        this.delayedDragging = true;
        return;
      }
      this.$nextTick(() => {
        this.delayedDragging = false;
      });
    }
  },
  emits: ["open-modal"]
};
</script>

<style>
@import "../assets/styles/board.scss";

.ghost {
  background-color: #d2b5ff;
}
</style>
