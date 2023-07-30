<template>
  <div class="mt-3">
    <b-form @submit="submitForm">
      <!-- language: select -->
      <b-form-group label="I'm learning:" label-for="language-select">
        <b-form-select
          id="language-select"
          :options="languageOptions"
          v-model="formData.language"
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
      <!-- topic: textarea -->
      <b-form-group label="Which topic:" label-for="topic-input">
        <b-form-textarea
          id="topic-input"
          placeholder="topic"
          v-model="formData.topic"
          rows="2"
          max-rows="4"
        ></b-form-textarea>
      </b-form-group>
      <!-- Submit Button -->
      <b-button type="submit" variant="primary">Submit</b-button>
    </b-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        language: "Dutch",
        topic: "",
        sentence_num: 3,
        level: "A1",
      },
      languageOptions: ["Dutch", "Japanese", "German"],
      levelOptions: ["A1", "A2", "B1", "B2"],
    };
  },
  methods: {
    submitForm() {
      // Handle form submission here
      console.log(this.formData);
      // You can perform further actions, such as sending the data to the server, etc.
    },
  },
};
</script>

<style>
/* Add custom styling here if needed */
</style>
