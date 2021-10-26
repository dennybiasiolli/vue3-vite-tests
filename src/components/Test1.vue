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
import {
  ref,
  onMounted,
  onBeforeUnmount,
  watch,
  toRefs,
  toRef,
  computed,
} from 'vue';

export default {
  props: {
    msg: {
      type: String,
      required: true,
    },
    prop1: {
      type: String,
      required: true,
    },
    prop2: {
      type: String,
      required: true,
    },
  },
  setup(props) {
    const { msg, prop1 } = toRefs(props);
    console.log(prop1);
    const prop2 = toRef(props, 'prop2');
    console.log(prop2);
    const counter = ref(0);
    const isEven = computed(() => counter.value % 2 === 0);
    const interval = ref(null);

    const handleStartCounter = () => {
      interval.value = setInterval(() => {
        counter.value++;
      }, 1000);
    };
    const handleStopCounter = () => {
      clearInterval(interval.value);
      interval.value = null;
    };

    onMounted(handleStartCounter);
    onBeforeUnmount(handleStopCounter);

    watch(counter, (newValue, oldValue) => {
      console.log(`counter changed from ${oldValue} to ${newValue}`);
    });
    watch(msg, (newValue, oldValue) => {
      console.log(`msg changed from "${oldValue}" to "${newValue}"`);
    });

    // anything returned here will be available for the rest of the component
    return {
      msg,
      counter,
      isEven,
      interval,
      handleStartCounter,
      handleStopCounter,
    };
  },
};
</script>
