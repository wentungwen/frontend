<template>
  <div class="mt-3">
    <b-form @submit="submitForm">
      <!-- language: select -->
      <b-form-group label="I'm learning:" label-for="language-select">
        <b-form-select
          id="language-select"
          :options="languageOptions"
          :value.sync="formData.language"
          placeholder="Choose a language"
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
          min="3"
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
export default {
  data() {
    return {
      formData: {
        language: "Dutch",
        topic: "home",
        sentence_num: 3,
        level: "A1",
      },
      languageOptions: ["Dutch", "Japanese", "German"],
      levelOptions: ["A1", "A2", "B1", "B2"],
      submit_msg: "",
    };
  },
  methods: {
    generate_conversation(payload) {
      axios
        .post("http://127.0.0.1:5000/conversations", payload)
        .then(() => {
          this.submit_msg = "Conversation generated";
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

<style>
/* Add custom styling here if needed */
</style>
