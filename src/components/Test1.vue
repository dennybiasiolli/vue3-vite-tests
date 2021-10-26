<script setup>
import {
  ref,
  onMounted,
  onBeforeUnmount,
  watch,
  computed,
} from 'vue';

const { msg, prop1, prop2 } = defineProps({
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
});

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
</script>

<template>
  <div>
    <h2>{{ msg }}</h2>
    <div>Counter: {{ counter }}</div>
    <div>Is even: {{ isEven }}</div>
    <button v-if="interval" @click="handleStopCounter">Stop counter</button>
    <button v-else @click="handleStartCounter">Start counter</button>
  </div>
</template>
