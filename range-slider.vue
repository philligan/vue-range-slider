<template>
  <div class="c-range-slider">
    <div class="c-range-slider__container">
      <span class="c-range-slider__rail-end c-range-slider__rail-end--min"></span>
      <span class="c-range-slider__rail-end c-range-slider__rail-end--max"></span>

      <span class="c-range-slider__scale">
        <span class="c-range-slider__mark"></span>
        <span class="c-range-slider__mark"></span>
        <span class="c-range-slider__mark"></span>
        <span class="c-range-slider__mark"></span>
        <span class="c-range-slider__mark"></span>
        <span class="c-range-slider__mark"></span>
        <span class="c-range-slider__mark"></span>
        <span class="c-range-slider__mark"></span>
        <span class="c-range-slider__mark"></span>
        <span class="c-range-slider__mark"></span>
        <span class="c-range-slider__mark"></span>
      </span>

      <span class="c-range-slider__root">
        <span class="c-range-slider__rail" ref="rail"></span>
        <span class="c-range-slider__track"></span>
        <span class="c-range-slider__thumb c-range-slider__thumb--min" tabindex="0"></span>
        <span
          @mousedown="dragHandler"
          :style="thumbMaxPosition"
          class="c-range-slider__thumb c-range-slider__thumb--max"
          tabindex="0"
        ></span>
      </span>
    </div>
    <!-- TODO: Make input type="hidden" -->
    <input :value="model" class="c-range-slider__model" type="readonly" />

    <button class="range-slider__submit btn btn-secondary" @click="submitRange">
      <slot name="button"></slot>
    </button>
  </div>
</template>

<script>
export default {
  props: {
    maxRange: {
      default: 100, // TODO: Remove
      type: Number,
      // required: true,
    },
    minRange: {
      default: 0, // TODO: Remove
      type: Number,
      // required: true,
    }
  },
  data() {
    return {
      activePointer: false,
      activePointerStartCoordX: 0,
      model: "",
      maxThumbPositionLeft: 0,
      pointerWidth: 20,
      scale: [],
      steps: 0,
    };
  },
  computed: {
    thumbMaxPosition() {
      return `left: ${this.maxThumbPositionLeft}%`;
    }
  },
  mounted() {
    document.addEventListener('mousemove', this.moveHandler);
    document.addEventListener('mouseup', this.dropHandler);

    this.maxThumbPositionLeft = 75; // TEMP
    this.scale = this.createScale();
    this.steps = this.calculateSteps();
  },
  methods: {
    calculateSteps() {
      return this.$refs.rail.clientWidth / this.scale.length
    },
    createScale() {
      return Array.from(Array(this.maxRange - this.minRange).keys());
    },
    dragHandler(e) {
      e.preventDefault();
      this.activePointer = true;
      console.log(`drag: ${e.pageX}`);
      // this.activePointerStartCoordX = e.pageX;
    },
    dropHandler(e) {
      if (this.activePointer) {
        this.activePointer = false;
        console.log(`drop: ${e.pageX}`);

  			// const	index = e.pageX - this.sliderLeft - (this.pointerWidth / 2);

        this.maxThumbPositionLeft = this.values.start * this.step + 'px';
        this.maxThumbPositionLeft = (this.values[activePointer] * this.step - (this.pointerWidth / 2)) + 'px';


        this.maxThumbPositionLeft = this.maxThumbPositionLeft - (this.activePointerStartCoordX - e.pageX);
        // this.maxThumbPositionLeft = this.maxThumbPositionLeft - (this.activePointerStartCoordX - e.pageX);
      }
    },
    moveHandler(e) {
      if (this.activePointer) {
        console.log(`move: ${e.pageX}`);
      }
    },
    submitRange() {
      this.$emit('submit');
    }
  }
}
</script>
