<template>
  <div class="dragContainer">
    <!-- TASK LIST FOR STATUS: TODO -->
    <div class="statusContainer">
      <h3>Todo - {{ taskList.todoList.length }}</h3>

      <draggable
        v-model="taskList.todoList"
        v-bind="dragOptions"
        :move="onMove"
      >
        <transition-group class="groupContainer">
          <div
            class="sortable"
            v-for="element in taskList.todoList"
            :key="element.id"
            @click="
              (isCardModalActive = true),
                (currTask = element),
                (currList = taskList.todoList),
                (currStatus = `todo`)
            "
          >
            {{ element.t_name }}
          </div>
        </transition-group>
      </draggable>

      <b-input
        placeholder="add new task"
        v-model="newtask"
        @keydown.native.enter="add(taskList.todoList, newtask)"
      ></b-input>
      <!-- <b-button @click="add(todoList, newtask)">add new task</b-button> -->
    </div>
    <!-- TASK LIST FOR STATUS: DOING -->
    <div class="statusContainer">
      <h3>Doing - {{ taskList.doingList.length }}</h3>
      <draggable
        v-model="taskList.doingList"
        v-bind="dragOptions"
        :move="onMove"
      >
        <transition-group class="groupContainer">
          <div
            class="sortable"
            v-for="element in taskList.doingList"
            :key="element.id"
            @click="
              (isCardModalActive = true),
                (currTask = element),
                (currList = taskList.doingList),
                (currStatus = `doing`)
            "
          >
            {{ element.t_name }}
          </div>
        </transition-group>
      </draggable>

      <b-input
        placeholder="add new task"
        v-model="newDoingTask"
        @keydown.native.enter="add(taskList.doingList, newDoingTask)"
      ></b-input>
      <!-- <b-button @click="add(doingList, newDoingTask)">add new task</b-button> -->
    </div>
    <!-- TASK LIST FOR STATUS: DONE -->
    <div class="statusContainer">
      <h3>Done - {{ taskList.doneList.length }}</h3>
      <draggable
        v-model="taskList.doneList"
        v-bind="dragOptions"
        :move="onMove"
      >
        <transition-group class="groupContainer">
          <div
            class="sortable"
            v-for="element in taskList.doneList"
            :key="element.t_name"
            @click="
              (isCardModalActive = true),
                (currTask = element),
                (currList = taskList.doneList),
                (currStatus = `done`)
            "
          >
            {{ element.t_name }}
          </div>
        </transition-group>
      </draggable>
      <b-input
        placeholder="add new task"
        v-model="newDoneTask"
        @keydown.native.enter="add(taskList.doneList, newDoneTask)"
      ></b-input>
      <!-- <b-button @click="add(doneList, newDoneTask)">add new task</b-button> -->
    </div>
    <!-- MODAL FOR TASKS -->
    <b-modal v-model="isCardModalActive" :width="640" scroll="keep">
      <div class="card">
        <div class="card-content">
          <div class="media">
            <div class="media-content">
              <h4>Status: {{ currStatus }}</h4>
              <b-input
                :value="currTask.t_name"
                class="title is-4"
                v-model="currTask.t_name"
              >
                <!-- @keydown.native.enter="update(currTask)" -->
              </b-input>
            </div>
          </div>

          <div class="content">
            <b-input
              :value="currTask.description"
              v-model="currTask.description"
            >
            </b-input>
            <b-button @click="handleDelete(currList, currTask)">
              delete task
            </b-button>
          </div>
        </div>
      </div>
    </b-modal>
    <!-- <modal-layout name="kgjerio"><h1>hello</h1></modal-layout> -->
    <!-- <status-column
      :list="taskList.todoList"
      :name="todo"
      @open-modal="iscardModalActive = true"
    ></status-column> -->
  </div>
</template>

<script>
import draggable from "vuedraggable";
import "buefy/dist/buefy.css";
import { v4 as uuidv4 } from "uuid";
// import statusColumn from "./statusColumn.vue";
// import ModalLayout from "./modalLayout.vue";
export default {
  components: {
    draggable,
    // ModalLayout,
    // statusColumn,
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

      taskList: {
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
      },
      editable: true,
      isDragging: false,
      delayedDragging: false,
    };
  },
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
    if (localStorage.taskList) {
      console.log(this.taskList);
      this.taskList = JSON.parse(localStorage.getItem(`taskList`) || "[]");
    }
    // if (localStorage.taskList.todo) {
    //   this.todoList = JSON.parse(
    //     localStorage.getItem(`taskList.todoList`) || "[]"
    //   );
    // }
    // if (localStorage.taskList.doing) {
    //   this.doingList = JSON.parse(
    //     localStorage.getItem(`taskList.doingList`) || "[]"
    //   );
    // }
    // if (localStorage.taskList.doing) {
    //   this.doneList = JSON.parse(
    //     localStorage.getItem(`taskList.doneList`) || "[]"
    //   );
    // }
  },
  watch: {
    // WATCH FOR CHANGES IN STATUS ARRAYS AND AUTOMATICALLY SAVE!
    // todoList(newstuff) {
    //   localStorage.setItem("todo", JSON.stringify(newstuff));
    // },
    // doingList(newstuff) {
    //   localStorage.setItem("doing", JSON.stringify(newstuff));
    // },
    // doneList(newstuff) {
    //   localStorage.setItem("done", JSON.stringify(newstuff));
    // },
    taskList: {
      deep: true,
      handler(newstuff) {
        console.log(newstuff);

        localStorage.setItem("taskList", JSON.stringify(newstuff));
      },
    },
    // taskList(newstuff) {

    //   console.log(newstuff);
    //   localStorage.setItem("taskList", JSON.stringify(newstuff));
    // },

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
