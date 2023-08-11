<template>
  <div>
    <b-button v-b-toggle.sidebar-1 class="m-2 mr-0 btn-light"
      ><b-icon-list></b-icon-list
    ></b-button>
    <b-sidebar id="sidebar-1" :title="sidebar_title" shadow>
      <b-col class="flex-grow-1">
        <hr />
        <b-icon-bank></b-icon-bank>
        <span class="pl-2">Saved conversations</span>
        <hr />
        <ul class="nav nav-pills flex-column mb-auto">
          <div v-if="conversations">
            <li
              class="nav-item"
              v-for="conversation in conversations"
              v-bind:key="conversation.uuid"
            >
              <b-link
                href="#"
                @click="load_conversation(conversation.uuid)"
                class="text-white text-decoration-none"
                active
              >
                <div class="nav-link active mb-2" aria-current="page">
                  <b-link
                    @click="delete_conversations(conversation.uuid)"
                    class="text-white"
                  >
                    <b-icon-x-circle-fill></b-icon-x-circle-fill>
                  </b-link>
                  <span class="pl-2">{{ conversation.topic }}</span>
                </div>
              </b-link>
            </li>
          </div>
          <div v-else>Add conversations</div>
        </ul>
      </b-col>
      <b-col class="position-absolute bottom-0">
        <hr />
        <b-link
          href="#"
          class="d-flex align-items-center link-dark text-decoration-none"
          id="dropdownUser2"
        >
          <img
            src="https://avatars.githubusercontent.com/u/78141260?v=4"
            alt=""
            width="32"
            height="32"
            class="rounded-circle mr-2"
          />
          <strong>username</strong>
        </b-link>
        <hr />
        <b-link href="#">Sign out</b-link>
      </b-col>
    </b-sidebar>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "NavBar",
  data() {
    return {
      sidebar_title: "Language Helper",
    };
  },
  props: {
    conversations: {
      type: Array,
    },
  },
  methods: {
    load_conversation(id) {
      this.$emit("load_conversation", id);
    },
    delete_conversations(id) {
      const url = "http://127.0.0.1:5000/delete_conversation/" + id;
      axios
        .delete(url)
        .then((res) => {
          if (res.status === 200) {
            this.$emit("conversation_deleted");
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>
<style>
.sidebar {
  height: 100vh;
  width: 200px;
}
</style>
