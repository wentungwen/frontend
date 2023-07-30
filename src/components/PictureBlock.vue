<template>
  <b-col class="d-flex flex-column">
    <b-button-group class="mt-4 px-4" role="group" aria-label="video-control">
      <b-button type="button" class="btn btn-primary">
        <b-icon icon="play-fill"></b-icon> Play
      </b-button>
      <b-button type="button" class="btn btn-secondary">
        <b-icon icon="stop-fill"></b-icon> Stop
      </b-button>
    </b-button-group>
    <b-carousel
      id="pic-carousel"
      v-model="slide"
      :interval="0"
      indicators
      background="#ababab"
      class="pic-carousel"
      @sliding-start="onSlideStart"
      @sliding-end="onSlideEnd"
    >
      <!-- Text slides with image -->
      <b-carousel-slide
        v-for="(message, idx) in messages"
        :text="message.content"
        :caption="message.sender"
        :key="idx"
        :img-src="require('../assets/person_a.png')"
      ></b-carousel-slide>
    </b-carousel>
    <!-- play and stop button -->
    <div class="col">
      <br />
      <b class="mt-3 fs-5"> English:{{ messages[slide].content }} </b>
    </div>
    <!-- Slide #: {{ slide }}<br />
    Sliding: {{ sliding }} -->
  </b-col>
</template>

<script>
export default {
  data() {
    return {
      slide: 0,
      sliding: null,
    };
  },
  props: {
    messages: {
      type: Array,
    },
  },
  methods: {
    onSlideStart(slide) {
      this.sliding = true;
      console.log(slide);
    },
    onSlideEnd(slide) {
      this.sliding = false;
      console.log(slide);
    },
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
