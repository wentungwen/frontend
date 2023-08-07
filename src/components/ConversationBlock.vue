<template>
  <div
    class="dialog-block mt-4 bg-light pb-3"
    aria-label="Toolbar with button groups"
  >
    <!-- Toolbar -->
    <b-button-toolbar class="toolbar">
      <b-button-group class="">
        <b-button @click="translate_btn(received_data)" variant="secondary"
          >Translate</b-button
        >
        <b-button @click="copy_btn" variant="secondary">Copy</b-button>
        <b-button @click="edit_btn" variant="secondary">Edit</b-button>
        <b-button @click="save_btn" variant="secondary">Save</b-button>
      </b-button-group>
    </b-button-toolbar>
    <!-- action alerts -->
    <b-alert :show="is_copied" class="m-3 alert-success">Copied!</b-alert>
    <!-- conversation editing block -->
    <div v-if="is_editing" class="dialog-content m-3">
      <p v-for="(msg, idx) in edited_conversations" :key="idx">
        <b>{{ msg.sender }}:</b>
        <b-form-input
          id="topic-input"
          v-model="msg.content"
          :required="true"
        ></b-form-input>
      </p>
      <b-button @click="edit_submit_btn" variant="secondary">Save</b-button>
    </div>
    <!-- conversation -->
    <div v-else class="dialog-content m-3" ref="copy_conversations">
      <div v-if="is_translation_shown">
        <p
          v-for="(msg, idx) in translated_conversations.conversations"
          :key="idx"
        >
          <b>{{ msg.sender }}:</b>
          {{ msg.content }}
        </p>
      </div>
      <div v-else>
        <p v-for="(msg, idx) in received_data.conversations" :key="idx">
          <b>{{ msg.sender }}:</b>
          {{ msg.content }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import { eventBus } from "@/main";
import axios from "axios";
export default {
  data() {
    return {
      is_copied: false,
      is_translation_shown: false,
      is_editing: false,
      translate_to_lan_code: "en",
      edited_conversations: [],
      translated_conversations: [],
      received_data: {
        lan_code: "nl",
        conversations: [
          {
            content: "(conversation block) het meisje.",
            sender: "A",
          },
          {
            content: "(conversation block) Volgens de man.",
            sender: "B",
          },
        ],
      },
    };
  },
  props: {
    conversation: {
      type: Object,
    },
  },
  methods: {
    edit_btn() {
      this.edited_conversations = this.received_data.conversations;
      this.is_editing = true;
    },
    edit_submit_btn() {
      this.received_data.conversations = this.edited_conversations;
      this.is_editing = false;
      this.translated_conversations = [];
    },
    translate_btn(received_data) {
      if (this.translated_conversations.length === 0) {
        const payload = {
          lan_code: received_data.lan_code,
          conversations: received_data.conversations,
          translate_to_lan_code: this.translate_to_lan_code,
        };
        // send to backend to translate
        axios
          .post("http://127.0.0.1:5000/translate", payload)
          .then((res) => {
            this.is_translation_shown = !this.is_translation_shown;
            this.translated_conversations = res.data;
            console.log(this.translated_conversations);
          })
          .catch((err) => {
            console.log(err);
          });
      } else {
        this.is_translation_shown = !this.is_translation_shown;
      }
    },
    copy_btn() {
      const conversationsDiv = this.$refs.copy_conversations;
      const range = document.createRange();
      range.selectNode(conversationsDiv);

      // Create a selection to work with the range
      const selection = window.getSelection();
      selection.removeAllRanges();
      selection.addRange(range);

      document.execCommand("copy");
      selection.removeAllRanges();
      this.is_copied = true;
      setTimeout(() => {
        this.is_copied = false;
      }, 2000);
    },
    save_btn() {
      console.log("Save function clicked");
    },
  },
  //   beforeUpdate() {
  //     if (this.is_editing == false) {
  //       // If data has been edited, emit the updated data
  //       eventBus.$emit("received_conversations", this.received_data);
  //     }
  //   },
  mounted() {
    eventBus.$on("generated_data", (data) => {
      if (data) {
        this.translated_conversations = [];
        this.edited_conversations = [];
        this.is_editing = false;
        this.received_data = data;
        console.log("ge", this.received_data);
        eventBus.$emit("received_data", this.received_data);
      }
    });
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
</style>
