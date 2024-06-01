<template>
  <form @submit.prevent="onSubmit">
    <input type="text" v-model.trim="label" />
    <button class="btn submitBtn" type="submit">&nbsp;+&nbsp;</button>
  </form>
</template>
<script>
export default {
  name: "ToDoForm",
  data() {
    return {
      label: "",
    };
  },
  methods: {
    onSubmit() {
      if (this.label !== "") {
        this.$emit("todo-added", this.label);
      }
      this.label = "";
    },
  },
};
</script>
<style scoped>
* {
  font-size: 1.2rem;
}
input {
  width: 60%;
  border-radius: 20px;
  border: 3px solid lightslategrey;
  padding: 2px 10px;
  padding-right: 25px;
}

input[type="text"]:focus {
  outline: 3px dashed lightsalmon;
}

.submitBtn {
  background-color: lightslategrey;
  border: none;
  color: white;
  font-weight: bold;
  border-radius: 50%;
  margin-left: -28px;
}
</style>
