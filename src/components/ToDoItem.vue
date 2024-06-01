<template>
  <div class="todos" v-if="!isEditing">
    <div class="todo-item">
      <input
        type="checkbox"
        :id="id"
        :checked="isDone"
        autocomplete="off"
        @change="$emit('checkbox-changed')"
      />
      <label :class="{ doneLabel: done }">{{ label }}</label>
    </div>
    <div class="btn-group">
      <button class="btn" @click="isEditing = true" ref="editBtn">Edit</button>
      <button class="btn warning" @click="deleteToDo">Delete</button>
    </div>
  </div>
  <to-do-item-edit
    v-else
    :id="id"
    :label="label"
    @edit-cancelled="editCancelled"
    @label-edited="labelEdited"
  ></to-do-item-edit>
</template>
<script>
import ToDoItemEdit from "./ToDoItemEdit.vue";
export default {
  name: "ToDoItem",
  components: {
    ToDoItemEdit,
  },
  data() {
    return {
      isEditing: false,
    };
  },
  props: {
    label: {
      type: String,
      required: true,
    },
    done: {
      type: Boolean,
      default: false,
    },
    id: {
      type: String,
      required: true,
    },
  },
  emits: {
    //warning vue3
    "item-deleted": null,
    "checkbox-changed": null,
    "label-edited": null,
  },
  methods: {
    deleteToDo() {
      this.$emit("item-deleted");
    },
    editCancelled() {
      this.isEditing = false;
      this.focusOnEditButton();
    },
    labelEdited(newLabel) {
      this.$emit("label-edited", newLabel);
      this.isEditing = false;
      this.focusOnEditButton();
    },
    focusOnEditButton() {
      this.$nextTick(() => {
        const editBtnRef = this.$refs.editBtn;
        editBtnRef.focus();
      });
    },
  },
  computed: {
    isDone() {
      return this.done;
    },
  },
};
</script>
<style scoped>
/* .todos {
  
} */

.todo-item {
  margin: auto;
  text-align: left;
}

.todo-item label {
  word-break: break-all;
  text-align: justify;
}

.doneLabel {
  text-decoration: line-through;
  text-decoration-color: lightsalmon;
  color: grey;
}
</style>
