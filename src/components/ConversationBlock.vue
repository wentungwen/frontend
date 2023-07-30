<template>
  <div
    class="dialog-block mt-4 bg-light pb-3"
    aria-label="Toolbar with button groups"
  >
    <!-- Toolbar -->
    <b-button-toolbar class="toolbar">
      <b-button-group class="">
        <b-button @click="translate" variant="secondary">Translate</b-button>
        <b-button @click="edit" variant="secondary">Edit</b-button>
        <b-button @click="copy" variant="secondary">Copy</b-button>
        <b-button @click="regenerate" variant="secondary">Regenerate</b-button>
        <b-button @click="save" variant="secondary">Save</b-button>
      </b-button-group>
    </b-button-toolbar>

    <!-- Dialog Content Goes Here -->
    <div class="dialog-content m-3">
      <p v-for="(msg, idx) in conversation.messages" :key="idx">
        <b>{{ msg.sender }}:</b>
        {{ msg.content }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: {
    conversation: {
      type: Object,
    },
  },
  methods: {
    translate() {
      console.log("Translate function clicked");
    },
    edit() {
      console.log("Edit function clicked");
    },
    copy() {
      console.log("Copy function clicked");
    },
    regenerate() {
      console.log("Regenerate function clicked");
    },
    save() {
      console.log("Save function clicked");
    },
  },
};
</script>

<style>
.toolbar {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #ccc;
}

/* Add additional styling for the dialog block and toolbar here */
</style>
