<template>
  <div id="app">
    <task></task>
  </div>
</template>
<script>
import Task from "./views/Task";

export default {
  name: "app",
  components: {
    Task
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  font-family: "Space Grotesk", sans-serif;
}

html {
  background-color: #f86457;
}

#app {
  background-color: #f86457;
}


</style>
