<template>
  <div class="statusContainer">
    <h3>{{ name }} - {{ alteredList.length }}</h3>
    <draggable v-model="alteredList" v-bind="dragOptions">
      <transition-group class="groupContainer">
        <div
          class="sortable"
          v-for="element in alteredList"
          :key="element.id"
          @click="$emit('open-modal', element, alteredList, name)"
        >
          {{ element.t_name }}
        </div>
      </transition-group>
    </draggable>
    <!-- ADD NEW TASK -->
    <b-input
      placeholder="add new task"
      v-model="newtask"
      @keydown.native.enter="add()"
    ></b-input>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import "buefy/dist/buefy.css";
import { v4 as uuidv4 } from "uuid";

export default {
  name: "StatusColumn",
  components: {
    draggable,
  },
  props: ["list", "name"],

  data() {
    return {
      alteredList: this.list,
      incomingList: "",
      newtask: "",
      isCardModalActive: false,
      editable: true,
      isDragging: false,
      delayedDragging: false,
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
    },
  },
  computed: {
    dragOptions() {
      return {
        animation: 250,
        group: "tasks",
        disabled: !this.editable,
        ghostClass: "ghost",
      };
    },

    serial() {
      let existing = JSON.parse(localStorage.getItem(`taskList`) || "[]");
      let i = 0;
      while (i < 100) {
        if (existing[i].name == this.name) {
          break;
        }
        i++;
      }
      return i;
    },
  },

  watch: {
    alteredList(newstuff) {
      let existing = JSON.parse(localStorage.getItem(`taskList`) || "[]");
      existing[this.serial].content = newstuff;
      localStorage.setItem("taskList", JSON.stringify(existing));
      console.log("watching component");
    },
    isDragging(newValue) {
      if (newValue) {
        this.delayedDragging = true;
        return;
      }
      this.$nextTick(() => {
        this.delayedDragging = false;
      });
    },
  },
  emits: ["open-modal"],
};
</script>

<style>
.ghost {
  background-color: rgb(253, 238, 255);
}
</style>
