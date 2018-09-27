<template>
  <div id="app">
    {{timeUnit}}
    {{currentSlide.start}}
      <Slide v-bind:html="currentSlide.html"/>
  </div>
</template>

<script>
import Slide from './components/Slide.vue'
import SlideData from './assets/slides.json'

var slides = [];
SlideData.forEach((s, i) => {
  var html = require(`./assets/slides/${s.Slide}.html`);

  slides.push({
    html: html,
    start: s.Start,
    id: i
  });
});

export default {
  name: 'app',
  components: {
    Slide
  },
  created: function() {
    setInterval(this.updateTime, 60/125 * 1000 / 2);
  },
  data: function() {
    return {
      slides: slides,
      timeUnit: 1,
      currentSlide: slides[0],
      currentSlideIndex: 0
    }
  },
  methods: {
    updateTime: function(v) {
      this.timeUnit++;

      if (this.slides.length < this.currentSlideIndex + 1) {
        return;
      }

      var nextSlide = this.slides[this.currentSlideIndex + 1];
      if (this.measureToTime(nextSlide.start) === this.timeUnit) {
        this.currentSlide = nextSlide;
        this.currentSlideIndex++;
      }
    },
    measureToTime: function(measure) {
      var parts = measure.split(":");
      var whoteNotes = this.getInt(parts[0]);
      var quarterNotes = this.getInt(parts[1]);
      var eigthNotes = this.getInt(parts[2]);

      return whoteNotes * 8 + quarterNotes * 2 + eigthNotes;
    },

    getInt(part) {
      if (isNaN(part)) {
        return 0;
      }

      return parseInt(part);
    }
  }
}

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
