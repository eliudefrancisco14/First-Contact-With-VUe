<template>
  <div class="fullname">
    <p>{{ fullName }}</p>
  </div>
</template>

<script>
export default {
  name: "FullName",
  data() {
    return {
      user: {
        firstName: "Eliude",
        lastName: "Francisco",
      },
    };
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
  },
};
</script>

<style scoped>
p {
  color: #fd4266;
  font-size: 20pt;
  font-weight: 20px;
}
</style>
