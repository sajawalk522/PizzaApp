<template>
  <div id="app">
    <Pizza />
  </div>
</template>

<script>
import Pizza from "./components/pizza.vue";

export default {
  name: "App",
  components: {
    Pizza,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Poppins:200i,400&display=swap");
body {
  padding: 0;
  margin: 0;
  
}
#app {
  height: 100vh;
  width: 100%;
  background-image: url("https://cdn.pixabay.com/photo/2016/03/05/09/22/eat-1237431_960_720.jpg");
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  font-family: "Poppins", sans-serif;
  font-size: 1rem;
  font-weight: 400;
  color: #fff;
}
</style>