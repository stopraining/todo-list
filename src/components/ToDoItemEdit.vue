<template>
  <form @submit.prevent="onSubmit">
    <div class="edit-item">
      <input
        type="text"
        ref="labelInput"
        autocomplete="off"
        v-model.lazy.trim="newLabel"
      />
    </div>
    <div class="btn-group">
      <button class="btn" type="button" @click="onCancel">Cancel</button>
      <button class="btn primary" type="submit">Save</button>
    </div>
  </form>
</template>
<script>
export default {
  name: "ToDoItemEdit",
  data() {
    return {
      newLabel: this.label,
    };
  },
  props: {
    label: {
      type: String,
      required: true,
    },
    id: {
      type: String,
      required: true,
    },
  },
  emits: {
    //warning vue3
    "edit-cancelled": null,
    "label-edited": null,
  },
  methods: {
    onCancel() {
      this.$emit("edit-cancelled");
    },
    onSubmit() {
      if ((this.newLabel !== this.label) & (this.newLabel !== "")) {
        this.$emit("label-edited", this.newLabel);
      }
    },
  },
  mounted() {
    const labelInputRef = this.$refs.labelInput;
    labelInputRef.focus();
  },
};
</script>

<style scoped>
.edit-item {
  margin: auto;
  text-align: left;
}

.edit-item input {
  width: 60%;
  border-radius: 20px;
  border: 1px solid lightslategrey;
  font-size: 1.2rem;
  padding: 0 10px;
}
</style>
