<template>
  <div id="app">
    <b-row>
      <b-col class="col-auto"> <NavBar :conversations="conversations" /></b-col>
      <b-col class="col-4">
        <GenerateForm />
        <ConversationBlock :conversation="conversations[active_conversation]" />
      </b-col>
      <b-col class="col piture-block">
        <PictureBlock :messages="conversations[active_conversation].messages" />
      </b-col>
    </b-row>
  </div>
</template>
<script>
import NavBar from "@/components/NavBar.vue";
import GenerateForm from "./components/GenerateForm.vue";
import ConversationBlock from "./components/ConversationBlock.vue";
import PictureBlock from "./components/PictureBlock.vue";
import axios from "axios";

export default {
  components: {
    NavBar,
    GenerateForm,
    ConversationBlock,
    PictureBlock,
  },
  data() {
    return {
      active_conversation: 1,
      conversations: [],
    };
  },
  methods: {
    get_conversations() {
      axios.get("http://127.0.0.1:5000/conversations").then((res) => {
        if (res.data) {
          this.conversations = res.data["data"];
        }
      });
    },
  },
  created() {
    this.get_conversations();
  },
};
</script>

<style scoped>
#app {
  height: 100vh;
}
body {
  min-height: 100vh;
}
.piture-block {
  max-width: 700px;
}
</style>
