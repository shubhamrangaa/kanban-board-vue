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
            :key="element.id"
            @click="
              (isCardModalActive = true),
                (currTask = element),
                (currTaskStatus = `todoList`)
            "
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
            :key="element.id"
            @click="
              (isCardModalActive = true),
                (currTask = element),
                (currTaskStatus = `doingList`)
            "
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
            @click="
              (isCardModalActive = true),
                (currTask = element),
                (currTaskStatus = `doneList`)
            "
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
    <!-- MODAL FOR TASKS -->
    <b-modal v-model="isCardModalActive" :width="640" scroll="keep">
      <div class="card">
        <div class="card-content">
          <div class="media">
            <div class="media-content">
              <b-input
                :value="currTask.t_name"
                class="title is-4"
                v-model="currTask.t_name"
                @keydown.native.enter="update(currTask)"
              >
              </b-input>
            </div>
          </div>

          <div class="content">
            <b-input
              :value="currTask.description"
              v-model="currTask.description"
              @keydown.native.enter="update(currTask)"
            >
            </b-input>
            <b-button @click="handleDelete(currTask)"> delete task </b-button>
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
import { v4 as uuidv4 } from "uuid";
// import ModalLayout from "./modalLayout.vue";
export default {
  components: {
    draggable,
    // ModalLayout,
  },

  data() {
    return {
      currTaskStatus: "",
      currTask: {},
      isCardModalActive: false,
      newtask: "",
      newDoingTask: "",
      newDoneTask: "",

      todoList: [
        {
          t_name: "laundry",
          id: uuidv4(),
          description: "I need to do my laundry",
        },
        {
          t_name: "clean",
          id: uuidv4(),
          description: "I need to do my cleanign",
        },
        { t_name: "code", id: uuidv4() },
      ],
      doingList: [
        { t_name: "run", id: uuidv4() },
        // { t_name: "bath", id: 5 },
        // { t_name: "vuejs", id: 6 },
      ],
      doneList: [
        { t_name: "shower", id: uuidv4() },
        // { t_name: "eat", id: 8 },
        // { t_name: "sleep", id: 9 },
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
        list.push({ t_name: task, id: uuidv4() });
        task = "";
      }
      // this.save();
    },
    update(currTask) {
      console.log(this.todoList);
      console.log(currTask.t_name);

      this.save();
      if (this.currTaskStatus === "todoList") {
        const taskIndex = this.todoList.findIndex(
          (element) => element.id == currTask.id
        );
        console.log(this.todoList[taskIndex].t_name);
      }
    },
    handleDelete(currTask) {
      console.log(this.currTaskStatus);
      if (this.currTaskStatus === "todoList") {
        const taskIndex = this.todoList.findIndex(
          (element) => element.id == currTask.id
        );
        console.log(this.todoList[taskIndex].t_name);
        this.todoList.splice(taskIndex, 1);
      } else if (this.currTaskStatus === "doingList") {
        const taskIndex = this.doingList.findIndex(
          (element) => element.id == currTask.id
        );
        console.log(this.doingList[taskIndex].t_name);
        this.doingList.splice(taskIndex, 1);
      } else if (this.currTaskStatus === "doneList") {
        const taskIndex = this.doneList.findIndex(
          (element) => element.id == currTask.id
        );
        console.log(this.doneList[taskIndex].t_name);
        this.doneList.splice(taskIndex, 1);
      }
    },
    save() {
      localStorage.setItem("todo", JSON.stringify(this.todoList));
      localStorage.setItem("doing", JSON.stringify(this.doingList));
      localStorage.setItem("done", JSON.stringify(this.doneList));
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
  mounted() {
    if (localStorage.todo) {
      this.todoList = JSON.parse(localStorage.getItem("todo") || "[]");
    }
    if (localStorage.doing) {
      this.doingList = JSON.parse(localStorage.getItem("doing") || "[]");
    }
    if (localStorage.doing) {
      this.doneList = JSON.parse(localStorage.getItem("done") || "[]");
    }
  },
  watch: {
    // WATCH FOR CHANGES IN STATUS ARRAYS AND AUTOMATICALLY SAVE!
    todoList(newstuff) {
      localStorage.setItem("todo", JSON.stringify(newstuff));
    },
    doingList(newstuff) {
      localStorage.setItem("doing", JSON.stringify(newstuff));
    },
    doneList(newstuff) {
      localStorage.setItem("done", JSON.stringify(newstuff));
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
};
</script>

<style>
@import "../assets/styles/board.css";
</style>
