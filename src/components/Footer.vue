<template>
  <footer>
    <p>copyright &copy; 2022 | @dabagire</p>
    <router-link to="/about">about</router-link>
  </footer>
</template>

<script>
export default {
  name: "Footer",
};
</script>

<style scoped>
a {
  color: #333;
}
footer {
  margin-top: 30px;
  text-align: center;
}
</style>
