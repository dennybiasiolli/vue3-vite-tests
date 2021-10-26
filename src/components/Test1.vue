<template>
  <div>
    <h2>{{ msg }}</h2>
    <div>Counter: {{ counter }}</div>
    <div>Is even: {{ isEven }}</div>
    <button v-if="interval" @click="handleStopCounter">Stop counter</button>
    <button v-else @click="handleStartCounter">Start counter</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      counter: 0,
      interval: null,
    };
  },
  props: {
    msg: {
      type: String,
      required: true,
    },
  },
  computed: {
    isEven() {
      return this.counter % 2 === 0;
    },
  },
  methods: {
    handleStartCounter() {
      this.interval = setInterval(() => {
        this.counter = this.counter + 1;
      }, 1000);
    },
    handleStopCounter() {
      clearInterval(this.interval);
      this.interval = null;
    },
  },
  mounted() {
    this.handleStartCounter();
  },
  beforeUnmount() {
    this.handleStopCounter();
  },
};
</script>
