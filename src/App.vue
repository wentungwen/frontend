<template>
  <div id="app">
    <b-row>
      <b-col class="col-auto"> <NavBar :conversations="conversations" /></b-col>
      <b-col class="col-4">
        <GenerateForm />
        <ConversationBlock :conversation="conversations[0]" />
      </b-col>
      <b-col class="col">
        <PictureBlock :messages="conversations[0].messages" />
      </b-col>
    </b-row>
  </div>
</template>
<script>
import NavBar from "@/components/NavBar.vue";
import GenerateForm from "./components/GenerateForm.vue";
import ConversationBlock from "./components/ConversationBlock.vue";
import PictureBlock from "./components/PictureBlock.vue";

export default {
  components: {
    NavBar,
    GenerateForm,
    ConversationBlock,
    PictureBlock,
  },
  data() {
    return {
      conversations: [
        {
          id: 1,
          date: "2019-05-04",
          title: "te laat",
          messages: [
            {
              sender: "A",
              content: "Ik heb de treinkaarten. Twee retourtjes naar Den Haag.",
            },
            {
              sender: "B",
              content:
                "Volgens de man achter het loket, vertrekt de trein over een kwartier van spoor vijf.",
            },
            {
              sender: "A",
              content: "Goed zo. Ik ga even naar de wc. Weet jij waar die is?",
            },
            {
              sender: "B",
              content: "Ja. Daar bij de ingang. Ik blijf hier bij de koffers.",
            },
          ],
        },
        {
          id: 2,
          date: "2019-05-05",
          title: "de klein tiran ",
          messages: [
            { sender: "A", content: "Mamma, waar is pappa?" },
            {
              sender: "B",
              content: "Op kantoor, natuurlijk. Ga maar even naar buiten.",
            },
            { sender: "A", content: "Speel met poesje in de tuin. " },
            {
              sender: "B",
              content: "Maar waarom niet op straat? Met de jongens.",
            },
          ],
        },
      ],
    };
  },
};
</script>

<style>
#app {
  height: 100vh;
}
body {
  min-height: 100vh;
}
</style>
