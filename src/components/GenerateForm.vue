<template>
  <div class="mt-3">
    <b-form @submit="submitForm">
      <!-- language: select -->
      <b-form-group label="I'm learning:" label-for="language-select">
        <b-form-select
          id="language-select"
          :options="languageOptions"
          v-model="formData.lan_code"
          >I'm learning:</b-form-select
        >
      </b-form-group>
      <!-- hardness level: range -->
      <b-form-group label="hard level" label-for="level-select">
        <b-form-select
          id="level-select"
          :options="levelOptions"
          v-model="formData.level"
          placeholder="Choose a level"
        ></b-form-select>
      </b-form-group>
      <!-- sentence numbers: number -->
      <b-form-group
        :label="'How many sentences: ' + formData.sentence_num"
        label-for="sentence-num-input"
      >
        <b-form-input
          id="sentence-num-input"
          placeholder="5"
          v-model="formData.sentence_num"
          type="range"
          min="1"
          max="10"
          step="1"
        ></b-form-input>
      </b-form-group>
      <!-- topic: text input -->
      <b-form-group label="Which topic:" label-for="topic-input">
        <b-form-input
          id="topic-input"
          placeholder="topic"
          v-model="formData.topic"
          :required="true"
        ></b-form-input>
      </b-form-group>
      <!-- Submit Button -->
      <b-button @submit="submitForm" type="submit" variant="primary"
        >Generate!</b-button
      >
    </b-form>
  </div>
</template>

<script>
import axios from "axios";
import { eventBus } from "@/main";
export default {
  data() {
    return {
      formData: {
        lan_code: "es",
        topic: "home",
        sentence_num: 1,
        level: "A1",
      },
      languageOptions: [
        { value: "nl", text: "Dutch" },
        { value: "es", text: "Spanish" },
        { value: "ja", text: "Japanese" },
        { value: "de", text: "German" },
      ],
      levelOptions: ["A1", "A2", "B1", "B2"],
      generated_data: {
        conversations: [],
        lan_code: null,
      },
    };
  },
  methods: {
    generate_conversation(payload) {
      axios
        .post("http://127.0.0.1:5000/conversations", payload)
        .then((response) => {
          this.generated_data.conversations = JSON.parse(response.data);
          this.generated_data.lan_code = this.formData.lan_code;
          eventBus.$emit("generated_data", this.generated_data);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    submitForm(evt) {
      evt.preventDefault();
      console.log(this.formData);
      this.generate_conversation(this.formData);
    },
  },
};
</script>
