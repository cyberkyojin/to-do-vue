<template>
  <div id="container">
    <ToDo />
  </div>
</template>

<script>
import ToDo from "./components/ToDo.vue";

export default {
  name: "App",
  components: {
    ToDo,
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  background-color: #141414;
}

#container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid #e858cb;
  padding: 30px;
  border-radius: 10px;
}
</style>
