<template>
  <div id="add-task">
    <form @submit.prevent="addTask">
      <div>
        <div class="d-flex">
          <input class="form-control mr-1"  v-model="inputTask" placeholder="Swimming" />
           <button  variant="primary" class="btn btn-primary btn-add">Add</button>
        </div>
      </div>
    </form>
  </div>
</template>
<script>
export default {
  name: "add-task",
  data() {
    return {
      inputTask: ""
    };
  },
  methods: {
    addTask() {
      if (this.inputTask !== "") {
        this.$emit("added-task", this.inputTask);
      }
      this.inputTask = "";
    }
  }
};
</script>

<style scoped>
#add-task {
  text-align: center;
  margin: 50px auto 50px;
}

.btn-add {
  width: 100px;
}

form div {
  margin: 1rem 0;
}

</style>
