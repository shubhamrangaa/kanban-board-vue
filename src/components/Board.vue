<template>
  <div class="dragContainer">
    <div v-for="list in taskList" :key="list.name">
      <status-column
        :list.sync="list.content"
        :name="list.name"
        @open-modal="openModal"
      ></status-column>
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
              </b-input>
            </div>
          </div>

          <div class="content">
            <b-input
              :value="currTask.description"
              v-model="currTask.description"
            >
            </b-input>
            <b-button @click="handleDelete()">
              delete task
            </b-button>
          </div>
        </div>
      </div>
    </b-modal>
    <b-input
      @keydown.native.enter="addColumn"
      placeholder="add new Status"
      v-model="newStatus"
      >add column</b-input
    >
  </div>
</template>

<script>
import "buefy/dist/buefy.css";
import { v4 as uuidv4 } from "uuid";
import StatusColumn from "./statusColumn.vue";

export default {
  components: {
    StatusColumn,
  },

  data() {
    return {
      currStatus: "",
      currList: "",
      currTask: {},
      isCardModalActive: false,
      newtask: "",
      newStatus: "",

      taskList: [
        {
          name: "Todo",
          content: [
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
        },
        {
          name: "Doing",
          content: [{ t_name: "run", id: uuidv4() }],
        },
        {
          name: "Done",
          content: [{ t_name: "shower", id: uuidv4() }],
        },
      ],
    };
  },
  methods: {
    openModal(element, list, listName) {
      // console.log(element);
      // console.log(list);
      // console.log(listName);
      this.isCardModalActive = true;
      this.currList = list;
      this.currTask = element;
      this.currStatus = listName;
    },
    handleDelete() {
      const taskIndex = this.currList.findIndex(
        (element) => element.id == this.currTask.id
      );
      console.log("deleting this task: ", this.currList[taskIndex].t_name);
      this.currList.splice(taskIndex, 1);
    },
    save() {
      console.log(this.taskList);
      localStorage.setItem("taskList", JSON.stringify(this.taskList));
    },
    addColumn() {
      this.taskList.push({
        name: this.newStatus,
        content: [{ t_name: "shower", id: uuidv4() }],
      });
    },
  },
  created() {
    if (localStorage.taskList) {
      console.log("created");
      // console.log(this.taskList);
      this.taskList = JSON.parse(localStorage.getItem(`taskList`) || "[]");
      // console.log(this.taskList);
    }
  },
  // mounted() {
  //   if (localStorage.taskList) {
  //     console.log("mounted");
  //     // console.log(this.taskList);
  //     this.taskList = JSON.parse(localStorage.getItem(`taskList`) || "[]");
  //     // console.log(this.taskList);
  //   }
  // },
  watch: {
    // WATCH FOR CHANGES IN STATUS ARRAYS AND AUTOMATICALLY SAVE!

    taskList: {
      deep: true,
      handler(newstuff) {
        console.log("Found new changes");
        localStorage.setItem("taskList", JSON.stringify(newstuff));
      },
    },
  },
};
</script>

<style>
@import "../assets/styles/board.css";
</style>
