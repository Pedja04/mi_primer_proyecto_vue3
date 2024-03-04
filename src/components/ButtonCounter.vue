<script setup>
import { computed } from "vue";
const props = defineProps(["counterValue"]);
const counter = computed(() => props.counterValue);
const classCounter = computed(() => {
  if (counter.value === 0) {
    return "zero";
  } else if (counter.value > 0) {
    return "positive";
  } else {
    return "negative";
  }
});
</script>
<template>
  <div class="container">
    <button :class="classCounter">{{ counterValue }}</button>
  </div>
</template>
