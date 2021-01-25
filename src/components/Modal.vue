<template>
  <div>
    <b-button id="show-btn" squared :variant="buttonColor" @click="showModal">{{
      id
    }}</b-button>
    <b-modal ref="my-modal" hide-footer>
      <div class="d-block text-center">
        <h3>{{ textWarning }}</h3>
      </div>
      <div class="button-footer">
        <b-button
          style="width:100px"
          variant="primary"
          inline
          @click="yesButton"
          >Yes</b-button
        >
        <b-button
          style="margin-left:5px; width:100px"
          variant="danger"
          inline
          @click="noButton"
          >No</b-button
        >
      </div>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: "modal",
  props: ["text-warning", "id", "button-color", "delete"],
  data() {
    return {};
  },
  methods: {
    showModal() {
      this.$refs["my-modal"].show();
    },
    yesButton() {
      this.$emit("callback", true);
      this.$refs["my-modal"].hide();
    },
    noButton() {
      this.$emit("callback", false);
      this.$refs["my-modal"].toggle("#toggle-btn");
    }
  }
};
</script>

<style scope>
.button-footer {
  text-align: right;
  margin-top: 50px;
}
</style>
