<template>
  <div class="dragContainer">
    <!-- TASK LIST FOR STATUS: TODO -->
    <div class="statusContainer">
      <h3>Todo - {{ todoList.length }}</h3>

      <draggable v-model="todoList" v-bind="dragOptions" :move="onMove">
        <transition-group class="groupContainer">
          <div
            class="sortable"
            v-for="element in todoList"
            :key="element.t_name"
            @click="(isCardModalActive = true), (currTask = element)"
          >
            {{ element.t_name }}
          </div>
        </transition-group>
      </draggable>

      <b-input
        placeholder="add new task"
        v-model="newtask"
        @keydown.native.enter="add(todoList, newtask)"
      ></b-input>
      <!-- <b-button @click="add(todoList, newtask)">add new task</b-button> -->
    </div>
    <!-- TASK LIST FOR STATUS: DOING -->
    <div class="statusContainer">
      <h3>Doing - {{ doingList.length }}</h3>
      <draggable v-model="doingList" v-bind="dragOptions" :move="onMove">
        <transition-group class="groupContainer">
          <div
            class="sortable"
            v-for="element in doingList"
            :key="element.t_name"
          >
            {{ element.t_name }}
          </div>
        </transition-group>
      </draggable>

      <b-input
        placeholder="add new task"
        v-model="newDoingTask"
        @keydown.native.enter="add(doingList, newDoingTask)"
      ></b-input>
      <!-- <b-button @click="add(doingList, newDoingTask)">add new task</b-button> -->
    </div>
    <!-- TASK LIST FOR STATUS: DONE -->
    <div class="statusContainer">
      <h3>Done - {{ doneList.length }}</h3>
      <draggable v-model="doneList" v-bind="dragOptions" :move="onMove">
        <transition-group class="groupContainer">
          <div
            class="sortable"
            v-for="element in doneList"
            :key="element.t_name"
          >
            {{ element.t_name }}
          </div>
        </transition-group>
      </draggable>
      <b-input
        placeholder="add new task"
        v-model="newDoneTask"
        @keydown.native.enter="add(doneList, newDoneTask)"
      ></b-input>
      <!-- <b-button @click="add(doneList, newDoneTask)">add new task</b-button> -->
    </div>
    <b-modal v-model="isCardModalActive" :width="640" scroll="keep">
      <div class="card">
        <div class="card-content">
          <div class="media">
            <div class="media-content">
              <b-input
                :value="currTask.t_name"
                class="title is-4"
                @keydown.native.enter="update(currTask)"
              >
                {{ currTask.t_name }}
              </b-input>
            </div>
          </div>

          <div class="content">
            <b-input :value="currTask.description">{{
              currTask.description
            }}</b-input>
            <small>11:09 PM - 1 Jan 2016</small>
          </div>
        </div>
      </div>
    </b-modal>
    <!-- <modal-layout name="kgjerio"><h1>hello</h1></modal-layout> -->
  </div>
</template>

<script>
import draggable from "vuedraggable";
import "buefy/dist/buefy.css";
// import ModalLayout from "./modalLayout.vue";
export default {
  components: {
    draggable,
    // ModalLayout,
  },
  data() {
    return {
      currTask: {},
      isCardModalActive: false,
      newtask: "",
      newDoingTask: "",
      newDoneTask: "",
      todoList: [
        { t_name: "laundry", id: 1, description: "I need to do my laundry" },
        { t_name: "clean", id: 2, description: "I need to do my cleanign" },
        { t_name: "code", id: 3 },
      ],
      doingList: [
        { t_name: "run", id: 4 },
        { t_name: "bath", id: 5 },
        { t_name: "vuejs", id: 6 },
      ],
      doneList: [
        { t_name: "shower", id: 7 },
        { t_name: "eat", id: 8 },
        { t_name: "sleep", id: 9 },
      ],
      editable: true,
      isDragging: false,
      delayedDragging: false,
    };
  },
  methods: {
    add(list, task) {
      if (task) {
        console.log(task);
        list.push({ t_name: task, id: 11 });
        this.task = "";
      }
    },
    // orderList() {
    //   this.list = this.list.sort((one, two) => {
    //     return one.order - two.order;
    //   });
    // },
    onMove({ relatedContext, draggedContext }) {
      const relatedElement = relatedContext.element;
      const draggedElement = draggedContext.element;
      return (
        (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
      );
    },
  },
  computed: {
    dragOptions() {
      return {
        animation: 0,
        group: "description",
        disabled: !this.editable,
        ghostClass: "ghost",
      };
    },
    listString() {
      return JSON.stringify(this.list, null, 2);
    },
    list2String() {
      return JSON.stringify(this.list2, null, 2);
    },
  },
  watch: {
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
};
</script>

<style>
@import "../assets/styles/board.css";
</style>
