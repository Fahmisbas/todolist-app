<template>
  <b-list-group id="item-task">
    <b-list-group-item variant="warning">
      <div class="row">
        <div class="col col-lg-3" style="text-align:left;">
          <input
            class="input-checkbox"
            type="checkbox"
            v-on:change="markDone"
            v-model="checkboxState"
          />
        </div>
        <div class="col col-lg-6">
          <div
            style="display:inline-block; overflow-wrap: break-word; width: 100%;"
            :class="{ 'is-done': task.isdone }"
          >
            {{ task.description }}
          </div>
        </div>
        <div class="col col-lg-3" style="text-align:right;">
          <span
            @click="expandDetail = !expandDetail"
            style="font-size:25px; cursor:pointer;"
          >
            {{ expandDetail ? "-" : "+" }}
          </span>
        </div>
      </div>
    </b-list-group-item>
    <div v-if="expandDetail">
      <div class="input-group">
        <input
          class="form-control mr-0"
          v-model="editedInput"
          placeholder="Edit Here"
        />
        <modal
          @callback="editTask"
          :id="'Apply'"
          :button-color="'primary'"
          :text-warning="'Are you sure you want to apply this change?'"
        ></modal>
        <modal
          @callback="deleteTask"
          :id="'Delete'"
          :button-color="'danger'"
          :text-warning="'Are you sure you want to delete this?'"
        ></modal>
      </div>
    </div>
  </b-list-group>
</template>

<script>
import Modal from "./Modal";

export default {
  name: "item-task",
  props: ["task"],
  components: {
    Modal
  },
  data() {
    return {
      checkboxState: this.task.isdone,
      editedInput: this.task.description,
      expandDetail: false,
      isApplied: false
    };
  },
  methods: {
    editTask(isApplied) {
      if (isApplied) {
        this.task.description = this.editedInput
        this.$emit("edited-task", this.task);
        if (this.isdone) {
          this.checkboxState = true;
        }
      }
    },
    deleteTask(isDeleteTask) {
      if (isDeleteTask) {
        this.$emit("delete-task", this.task.id);
      }
    },
    markDone() {
      this.task.isdone = !this.task.isdone;
      this.$emit("mark-done", this.task);
    }
  }
};
</script>

<style scoped>
.expand:hover {
  cursor: move;
}

#item-task {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 10px;
  text-align: left;
  background-color: white;
  width: 100%;
  margin-top: 15px;
  max-width: 30rem;
}

#item-task:hover {
  cursor: move;
}

.is-done {
  text-decoration: line-through;
  color: red;
}

.btn-expand {
  text-align: right;
  display: inherit;
}
</style>
