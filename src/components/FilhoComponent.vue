<template>
  <div :class="'alert alert-' + styleprop">
    <slot name="paragraph" />
    <button @click="onClickk" class="btn-primary">X</button>
  </div>
</template>

<script>
export default {
  name: "FilhoComponent",
  props: ["styleprop"],
  method: {
    onClickk() {
      this.$emit('onClose')
      console.log("Clicou");
    },
  },
};
</script>
<style scoped>
.alert {
  color: #fff;
  border-radius: 20px;
  margin: 0 auto;
  padding: 20px 0;
}
.alert-primary {
  background-color: #0d6efd;
  color: #fff;
}
.alert-danger {
  background-color: #dc3545;
  color: #fff;
}
.alert-success {
  background-color: #198754;
  color: #fff;
}
button {
  font-size: 20px;
  border: none;
  outline: none;
  border-radius: 6px;
  cursor: pointer;
  color: #fff;
}
button:active {
  box-shadow: 0px 0px 0px 2px #ffffff48;
}
.btn-primary {
  background-color: #0d6efd;
}
.btn-primary:active,
.btn-primary:hover
.btn-primary:focus {
  background-color: #3988ff;
  box-shadow: 0px 0px 0px 2px #ffffff48;
}
</style>
