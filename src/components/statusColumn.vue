<template>
  <div class="statusContainer">
    <!-- <h3>Todo - {{ taskList.todoList.length }}</h3> -->
    <h3>Todo - {{ list.length }}</h3>

    <draggable v-model="list" v-bind="dragOptions" :move="onMove">
      <transition-group class="groupContainer">
        <div
          class="sortable"
          v-for="element in list"
          :key="element.id"
          @click="
            $emit(
              'open-modal',
              (currTask = element),
              (currList = list),
              (currStatus = name)
            )
          "
        >
          {{ element.t_name }}
        </div>
      </transition-group>
    </draggable>

    <b-input
      placeholder="add new task"
      v-model="newtask"
      @keydown.native.enter="add(list, newtask)"
    ></b-input>
    <!-- <b-button @click="add(todoList, newtask)">add new task</b-button> -->
  </div>
</template>

<script>
import draggable from "vuedraggable";
import "buefy/dist/buefy.css";
import { v4 as uuidv4 } from "uuid";

export default {
  components: {
    draggable,
  },
  data() {
    return {
      currStatus: "",
      currList: "",
      currTask: {},
      isCardModalActive: false,
      newtask: "",
      newDoingTask: "",
      newDoneTask: "",
    };
  },
  props: ["list", "name"],

  methods: {
    add(list, task) {
      if (task) {
        console.log(task);
        console.log(list);
        list.push({ t_name: task, id: uuidv4() });
        task = "";
      }
      // if (list === "todoList") {
      //   console.log(list);
      //   this.taskList.todoList.push({ t_name: task, id: uuidv4() });
      //   task = "";
      // }
      // this.save();
    },
    handleDelete(currList, currTask) {
      console.log(this.currList);
      console.log(currTask);
      console.log(currList);

      const taskIndex = currList.findIndex(
        (element) => element.id == currTask.id
      );
      console.log(currList[taskIndex].t_name);
      currList.splice(taskIndex, 1);

      // if (this.currTaskStatus === "todoList") {
      //   const taskIndex = this.taskList.todoList.findIndex(
      //     (element) => element.id == currTask.id
      //   );
      //   console.log(this.todoList[taskIndex].t_name);
      //   this.todoList.splice(taskIndex, 1);
      // } else if (this.currTaskStatus === "doingList") {
      //   const taskIndex = this.doingList.findIndex(
      //     (element) => element.id == currTask.id
      //   );
      //   console.log(this.doingList[taskIndex].t_name);
      //   this.doingList.splice(taskIndex, 1);
      // } else if (this.currTaskStatus === "doneList") {
      //   const taskIndex = this.doneList.findIndex(
      //     (element) => element.id == currTask.id
      //   );
      //   console.log(this.doneList[taskIndex].t_name);
      //   this.doneList.splice(taskIndex, 1);
      // }
    },
    save() {
      localStorage.setItem("taskList", JSON.stringify(this.taskList));
    },

    onMove({ relatedContext, draggedContext }) {
      const relatedElement = relatedContext.element;
      const draggedElement = draggedContext.element;
      return (
        (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
      );
    },
  },
  // computed: {
  //   dragOptions() {
  //     return {
  //       animation: 0,
  //       group: "description",
  //       disabled: !this.editable,
  //       ghostClass: "ghost",
  //     };
  //   },
  //   listString() {
  //     return JSON.stringify(this.list, null, 2);
  //   },
  //   list2String() {
  //     return JSON.stringify(this.list2, null, 2);
  //   },
  // },

  // watch: {
  //   // WATCH FOR CHANGES IN STATUS ARRAYS AND AUTOMATICALLY SAVE!
  //   // todoList(newstuff) {
  //   //   localStorage.setItem("todo", JSON.stringify(newstuff));
  //   // },
  //   // doingList(newstuff) {
  //   //   localStorage.setItem("doing", JSON.stringify(newstuff));
  //   // },
  //   // doneList(newstuff) {
  //   //   localStorage.setItem("done", JSON.stringify(newstuff));
  //   // },
  //   // taskList: {
  //   //   deep: true,
  //   //   handler(newstuff) {
  //   //     console.log(newstuff);

  //   //     localStorage.setItem("taskList", JSON.stringify(newstuff));
  //   //   },
  //   // },
  //   // taskList(newstuff) {

  //   //   console.log(newstuff);
  //   //   localStorage.setItem("taskList", JSON.stringify(newstuff));
  //   // },

  //   isDragging(newValue) {
  //     if (newValue) {
  //       this.delayedDragging = true;
  //       return;
  //     }
  //     this.$nextTick(() => {
  //       this.delayedDragging = false;
  //     });
  //   },
  // },
  emits: ["open-modal"],
};
</script>
