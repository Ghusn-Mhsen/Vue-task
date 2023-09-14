<template>
  <form class="stack-small" @submit.prevent="onSubmit">
    <div>
      <label class="edit-label"> Edit Name for &quot;{{ `Name : ${label   }   ` }}&quot;</label>
      <input
        :id="id"
        ref="labelInput"
        type="text"
        autocomplete="off"
        v-model.lazy.trim="newLabel" />

        <label class="edit-label"> Edit code for &quot;{{ `Code : ${label1   }   ` }}&quot;</label>
      <input
        :id="id"
        ref="labelInput"
        type="text"
        autocomplete="off"
        v-model.lazy.trim="newLabel1" />
    </div>
    <div class="btn-group">
      <button type="button" class="btn" @click="onCancel">
        Cancel
        <span class="visually-hidden">editing {{ label }}</span>
      </button>
      <button type="submit" class="btn btn__primary">
        Save
        <span class="visually-hidden">edit for :  {{ label }}</span>
      </button>
    </div>
  </form>
</template>
<script>
export default {
  props: {
    label: {
      type: String,
      required: true,
    },
      label1: {
      type: String,
      required: true,
    },
    id: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      newLabel: this.label,
      newLabel1:this.label1,
    };
  },
  methods: {
    onSubmit() {
      if (this.newLabel!== this.label || this.newLabel1 !== this.label1) {
        this.$emit("item-edited", {name:this.newLabel,code:this.newLabel1});
      }
    },
    onCancel() {
      this.$emit("edit-cancelled");
    },
  },
  mounted() {
    const labelInputRef = this.$refs.labelInput;
    labelInputRef.focus();
  },
};
</script>
<style scoped>
.edit-label {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #0b0c0c;
  display: block;
  margin-bottom: 5px;
}
input {
  display: inline-block;
  margin-top: 0.4rem;
  width: 100%;
  min-height: 4.4rem;
  padding: 0.4rem 0.8rem;
  border: 2px solid #565656;
}
form {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
form > * {
  flex: 0 0 100%;
}
</style>