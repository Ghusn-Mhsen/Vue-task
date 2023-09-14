<template>
  <form @submit.prevent="onSubmit">
    <h2 class="label-wrapper">
      <label for="new-todo-input" class="label__lg">
        Enter City Info ?
      </label>
    </h2>
    <input
      type="text"
      id="new-todo-input"
      name="new-todo"
      placeholder="City Name"
      autocomplete="off"
      v-model.lazy.trim="label"
      class="input__lg" />
      <input
      type="text"
      id="new-todo-input1"
      name="new-todo"
      placeholder="City Code"
      autocomplete="off"
      v-model.lazy.trim="label1"
      class="input__lg" />
    <button type="submit" class="btn btn__primary btn__lg">Add</button>
  </form>
  
</template>

<script>
export default {

  methods: {

    onSubmit() {


      if (this.label === ""||this.label1==="") {
        return;
      }
      
      this.$emit("todo-added", {name:this.label,code:this.label1});
      this.label = "";
      this.label1="";
    },
  },
  data() {
    return {
      label: "",
      label1:""
    };
  },
};
</script>