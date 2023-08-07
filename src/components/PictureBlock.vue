<template>
  <b-col class="d-flex flex-column">
    <!-- play and stop button -->
    <b-button-group class="mt-4 px-4" role="group" aria-label="video-control">
      <b-button type="button" class="btn btn-primary" @click="play_slides">
        <b-icon icon="play-fill"></b-icon> Play
      </b-button>
      <b-button type="button" class="btn btn-secondary" @click="stop_slides">
        <b-icon icon="stop-fill"></b-icon> Stop
      </b-button>
      <b-button type="button" class="btn btn-secondary" @click="stop_slides">
        Slower
      </b-button>
    </b-button-group>
    <!-- <div>
      <b-button
        @click="
          speak_text(
            received_data.conversations[current_slide].content,
            received_data.lan_code,
            speech_speed
          )
        "
        >Speak "Hello, world!" in English (Slower)</b-button
      >
      <b-button @click="stop_speaking">Stop</b-button>
    </div> -->
    <!-- Text slides with image -->
    <b-carousel
      fade
      controls
      indicators
      v-model="current_slide"
      :interval="slides_speed"
      background="#ababab"
      class="pic-carousel"
      @sliding-start="onSlideStart"
      @sliding-end="onSlideEnd"
    >
      <b-carousel-slide
        v-for="(conversation, idx) in received_data.conversations"
        :text="conversation.content"
        :key="idx"
        :current_slide="idx"
        :img-src="slide_image(conversation.sender)"
      ></b-carousel-slide>
    </b-carousel>
    Current_slide #: {{ current_slide }}<br />
    Sliding: {{ sliding }}
  </b-col>
</template>

<script>
import { eventBus } from "@/main";
export default {
  data() {
    return {
      speech_speed: 0.8,
      speech_utterance: null,
      current_slide: 0,
      sliding: null,
      slides_speed: 0,
      slide_imgs: {
        A: require("../assets/person_a.png"),
        B: require("../assets/person_b.png"),
      },
      received_data: {
        lan_code: "nl",
        conversations: [
          {
            content: "het meisje.",
            sender: "A",
          },
          {
            content: "Volgens de man.",
            sender: "B",
          },
          {
            content: "2. het meisje is klein.",
            sender: "A",
          },
          {
            content: "2. Volgens de man is grote.",
            sender: "B",
          },
        ],
      },
    };
  },
  props: {
    messages: {
      type: Array,
    },
  },
  methods: {
    async play_slides() {
      this.sliding = true;
      this.slides_speed = 1000;
      for (
        let i = this.current_slide;
        i < this.received_data.conversations.length;
        i++
      ) {
        console.log("Playing slide:", i);
        const current_content = this.received_data.conversations[i].content;
        this.speak_text(
          current_content,
          this.received_data.lan_code,
          this.speech_speed
        );

        await new Promise((resolve) => {
          this.speech_utterance.onend = resolve; // Resolve the promise when the speech is finished
        });

        console.log("Finished slide:", i);
        if (this.sliding === false) {
          break; // Stop the loop if sliding is set to false (stop button clicked)
        }
      }
      this.sliding = false; // Set sliding to false after all slides are played
    },

    async exampleFunction() {
      try {
        const result = await new Promise((reolve, reject) => {
          setTimeout(() => {
            reolve("Operation completed successfully!");
          }, 2000);
        });
        console.log(result);
      } catch (error) {
        console.error("Error:", error);
      }
    },

    stop_slides() {
      this.slides_speed = 0;
    },
    speak_text(text, languageCode, rate) {
      this.speech_utterance = new SpeechSynthesisUtterance(text);
      this.speech_utterance.lang = languageCode;
      this.speech_utterance.rate = rate; // Adjust the speaking speed
      window.speechSynthesis.speak(this.speech_utterance);
    },
    stop_speaking() {
      if (this.speech_utterance) {
        window.speechSynthesis.cancel();
      }
    },
    onSlideStart(slide) {
      this.sliding = true;
      console.log(slide);
    },
    onSlideEnd(slide) {
      console.log("end", slide);
      this.sliding = false;
      this.slides_speed = 0;
    },
  },
  computed: {
    slide_image() {
      return (sender) => {
        return this.slide_imgs[sender];
      };
    },
  },
  watch: {
    // current_slide(slide) {
    //   console.log(slide);
    //   if (slide == this.received_data.conversations.length - 1) {
    //     this.stop_slides();
    //     console.log("end slides");
    //   }
    // },
  },
  mounted() {
    eventBus.$on("received_data", (data) => {
      this.received_data = data;
    });
  },
};
</script>
<style>
.pic-carousel {
  width: 90%;
  font-size: 1.5rem;
  text-shadow: 1px 1px 2px #333;
  margin: 20px auto;
  border-radius: 1rem;
}
.carousel-caption {
  background: #0000008f;
  border-radius: 1rem;
  font-size: 1.5rem;
  right: 3% !important;
  left: 3% !important;
  padding: 10px !important;
}
</style>
