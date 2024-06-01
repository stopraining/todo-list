<template>
  <h3>My To-Do List</h3>
  <div>
    <to-do-form @todo-added="addTodo"></to-do-form>
    <button class="filterBtn" @click="filterAll">
      <span v-show="showMode === 'all'">▶ </span>
      全部({{ todoItems.length }})
    </button>
    <button class="filterBtn" @click="filterUndone">
      <span v-show="showMode === 'undone'">▶ </span>
      進行中({{ todoItems.length - doneItemCount }})
    </button>
    <button class="filterBtn" @click="filterDone">
      <span v-show="showMode === 'done'">▶ </span>
      已完成({{ doneItemCount }})
    </button>

    <button class="cleanBtn" @click="deleteAll">全部清除</button>
  </div>
  <div class="all" v-show="showMode === 'all'">
    <!-- <ul> -->
    <transition-group tag="ul" class="number-list" name="list">
      <li v-for="item in todoItems" :key="item.id">
        <to-do-item
          :label="item.label"
          :done="item.done"
          :id="item.id"
          @checkbox-changed="updateState(item.id)"
          @item-deleted="deleteToDo(item.id)"
          @label-edited="editTodo(item.id, $event)"
        ></to-do-item>
      </li>
    </transition-group>
    <!-- </ul> -->
  </div>
  <div class="undone" v-show="showMode === 'undone'">
    <!-- <ul> -->
    <transition-group tag="ul" class="number-list" name="list">
      <li v-for="item in todoItems" :key="item.id">
        <to-do-item
          v-if="!item.done"
          :label="item.label"
          :done="item.done"
          :id="item.id"
          @checkbox-changed="updateState(item.id)"
          @item-deleted="deleteToDo(item.id)"
          @label-edited="editTodo(item.id, $event)"
        ></to-do-item>
      </li>
    </transition-group>
    <!-- </ul> -->
  </div>
  <div class="done" v-show="showMode === 'done'">
    <!-- <ul> -->
    <transition-group tag="ul" class="number-list" name="list">
      <li v-for="item in todoItems" :key="item.id">
        <to-do-item
          v-if="item.done"
          :label="item.label"
          :done="item.done"
          :id="item.id"
          @checkbox-changed="updateState(item.id)"
          @item-deleted="deleteToDo(item.id)"
          @label-edited="editTodo(item.id, $event)"
        ></to-do-item>
      </li>
    </transition-group>
    <!-- </ul> -->
  </div>
</template>

<script>
import ToDoItem from "./components/ToDoItem.vue";
import ToDoForm from "./components/ToDoForm.vue";
import uniqueId from "lodash.uniqueid";

export default {
  name: "App",
  components: {
    ToDoItem,
    ToDoForm,
  },
  data() {
    return {
      todoItems: [
        // {
        //   id: uniqueId("todo-"),
        //   label: "請從上方的輸入框新增代辦事項!",
        //   done: true,
        // },
        // {
        //   id: uniqueId("todo-"),
        //   label: "Please input some todos on the input box above.",
        //   done: false,
        // },
      ],
      showMode: "all",
    };
  },
  methods: {
    filterAll() {
      this.showMode = "all";
    },
    filterUndone() {
      this.showMode = "undone";
    },
    filterDone() {
      this.showMode = "done";
    },
    updateState(id) {
      const toDoToUpdate = this.todoItems.find((item) => item.id === id);
      toDoToUpdate.done = !toDoToUpdate.done;
      this.localStorageUpdate();
    },
    addTodo(label) {
      this.todoItems.push({
        id: uniqueId("todo-"),
        label: label,
        done: false,
      });
      this.localStorageUpdate();
    },
    deleteToDo(id) {
      const deletedIndex = this.todoItems.findIndex((item) => item.id === id);
      this.todoItems.splice(deletedIndex, 1);
      this.localStorageUpdate();
    },
    editTodo(id, newLabel) {
      const editedIndex = this.todoItems.find((item) => item.id === id);
      editedIndex.label = newLabel;
      this.localStorageUpdate();
    },
    deleteAll() {
      this.todoItems = [];
      this.localStorageUpdate();
    },
    localStorageUpdate() {
      window.localStorage.setItem("myToDo", JSON.stringify(this.todoItems));
    },
  },
  computed: {
    doneItemCount() {
      let doneItemNum = this.todoItems.filter((item) => item.done).length;
      return doneItemNum;
    },
  },
  mounted() {
    //localStorage
    let myToDo = window.localStorage.getItem("myToDo");
    if (!myToDo) {
      window.localStorage.setItem("myToDo", JSON.stringify([]));
      myToDo = window.localStorage.getItem("myToDo");
    }
    this.todoItems = JSON.parse(myToDo);
  },
};
</script>

<style>
#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale; */
  text-align: center;
  color: #2c3e50;
  margin: auto;
  margin-top: 30px;
  max-width: 30rem;
  box-shadow: 2px 2px 10px gray;
  padding: 10px;
  font-size: 1.2rem;
}

ul > li {
  list-style-type: none;
  margin-left: -40px;
}

.btn {
  padding: 0.2rem 0.2rem 0.2rem;
  border: 1px solid lightgray;
  cursor: pointer;
}

.btn-group {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 5px;
  margin-bottom: 5px;
}

.btn-group button {
  width: 50%;
}

.filterBtn,
.cleanBtn {
  width: 90px;
  padding: 0.6rem 0.2rem 0.2rem;
  background: none;
  border: none;
  margin-left: 10px;
  cursor: pointer;
}

.cleanBtn:hover:before {
  content: "X";
  color: red;
}

.primary {
  background-color: lightslategrey;
  color: white;
}
.warning {
  background-color: lightsalmon;
  color: white;
}
/* animation */
.list-enter-active {
  transition: opacity 0.3s, transform 0.4s;
}

.list-leave-active,
.list-move {
  transition: opacity 0.3s, transform 0.2s;
}

/* .list-leave-active {
  position: absolute;
} */

.list-enter-from {
  opacity: 0;
  transform: translateY(-10px);
}

.list-leave-to {
  opacity: 0;
  transform: translateX(20px);
}
/**/

@media screen and (max-width: 500px) {
  #app {
    box-shadow: none;
    font-size: 1rem;
  }
}

@media screen and (min-width: 1000px) {
  #app {
    max-width: 600px;
  }
}
</style>
