<template>
  <div id="app">
    <form-table />
  </div>
</template>

<script>
import Form_Table from "./components/Form_Table";

export default {
  name: "App",
  components: {
    "form-table": Form_Table
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
