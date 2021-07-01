<template>
  <client-only>
    <c-flex
      wrap="nowrap"
      overflowX="scroll"
      pb="2rem"
      class="boardContainer"
      align="start"
    >
      <c-box v-for="list in taskList" :key="list.name">
        <!-- class="groupColumnContainer" -->
        <status-column
          :list.sync="list.content"
          :name="list.name"
          @open-modal="openModal"
        ></status-column>
      </c-box>
      <!-- TASK MODAL -->
      <c-modal
        :is-open="isOpen"
        :on-close="close"
        :closeOnOverlayClick="true"
        is-centered
      >
        <c-modal-content ref="content">
          <c-modal-header>
            <c-badge variant-color="indigo" mb="5px" font-size=".85em">
              {{ currStatus }}
            </c-badge>
          </c-modal-header>
          <c-modal-close-button />
          <c-modal-body>
            <c-box>
              <c-input
                v-model="currTask.t_name"
                mb="1rem"
                variant="unstyled"
                font-size="2xl"
              >
              </c-input>

              <c-textarea
                v-model="currTask.description"
                placeholder="Add a description"
              >
              </c-textarea>
            </c-box>
          </c-modal-body>
          <c-modal-footer>
            <c-button variant-color="gray" mr="3">
              Save
            </c-button>
            <c-button variant-color="red" mr="3" @click="handleDelete()">
              Delete
            </c-button>
            <!-- <c-button @click="close">Cancel</c-button> -->
          </c-modal-footer>
        </c-modal-content>
        <c-modal-overlay />
      </c-modal>
      <c-input
        @keydown.native.enter="addColumn"
        placeholder="+ Group"
        v-model="newStatus"
        minW="200px"
        variant="unstyled"
      ></c-input>
    </c-flex>
  </client-only>
</template>

<script>
import "buefy/dist/buefy.css";
import { v4 as uuidv4 } from "uuid";
import StatusColumn from "./statusColumn.vue";
export default {
  components: {
    StatusColumn
  },
  data() {
    return {
      currStatus: "",
      currList: "",
      currTask: {},
      isOpen: false,
      newtask: "",
      newStatus: "",
      taskList: [
        {
          name: "Todo",
          content: [
            {
              t_name: "laundry",
              id: uuidv4(),
              description: "I need to do my laundry"
            },
            {
              t_name: "clean",
              id: uuidv4(),
              description: "I need to do my cleanign"
            },
            { t_name: "code", id: uuidv4() }
          ]
        },
        {
          name: "Doing",
          content: [{ t_name: "run", id: uuidv4() }]
        },
        {
          name: "Done",
          content: [{ t_name: "shower", id: uuidv4() }]
        }
      ]
    };
  },
  methods: {
    openModal(element, list, listName) {
      // console.log(element);
      // console.log(list);
      // console.log(listName);
      this.isOpen = true;

      this.currList = list;
      this.currTask = element;
      this.currStatus = listName;
    },
    close() {
      this.isOpen = false;
    },
    handleDelete() {
      const taskIndex = this.currList.findIndex(
        element => element.id == this.currTask.id
      );
      console.log("deleting this task: ", this.currList[taskIndex].t_name);
      this.currList.splice(taskIndex, 1);
      this.isOpen = false;
    },
    save() {
      console.log(this.taskList);
      //   localStorage.setItem("taskList", JSON.stringify(this.taskList));
    },
    addColumn() {
      this.taskList.push({
        name: this.newStatus,
        content: [{ t_name: "shower", id: uuidv4() }]
      });
      this.newStatus = "";
    }
  },
  created() {
    if (process.browser) {
      if (localStorage.taskList) {
        console.log("created");
        // console.log(this.taskList);
        this.taskList = JSON.parse(localStorage.getItem(`taskList`) || "[]");
        // console.log(this.taskList);
      } else {
        localStorage.setItem("taskList", JSON.stringify(this.taskList));
      }
    }
  },
  // mounted() {
  //   if (process.browser) {
  //     if (localStorage.taskList) {
  //       console.log("mounted");
  //       // console.log(this.taskList);
  //       this.taskList = JSON.parse(localStorage.getItem(`taskList`) || "[]");
  //       // console.log(this.taskList);
  //     }
  //   }
  // },
  watch: {
    // WATCH FOR CHANGES IN STATUS ARRAYS AND AUTOMATICALLY SAVE!
    taskList: {
      deep: true,
      handler(newstuff) {
        console.log("Fsound new changes");
        localStorage.setItem("taskList", JSON.stringify(newstuff));
      }
    }
  }
};
</script>

<style>
@import "../assets/styles/board.css";
</style>
