<script setup>
import { computed } from "vue";
const emit = defineEmits(["next", "prev"]);
const props = defineProps(["inicio", "fin", "maxLength"]);
const bloquearPrev = computed(() => props.inicio <= 0);
const bloquearNext = computed(() => props.fin >= props.maxLength);
</script>
<template>
  <div class="btn-group" role="group" aria-label="Basic example">
    <button
      @click="emit('prev')"
      type="button"
      class="btn btn-outline-primary"
      :disabled="bloquearPrev"
    >
      Previus
    </button>
    <button
      @click="emit('next')"
      type="button"
      class="btn btn-outline-primary"
      :disabled="bloquearNext"
    >
      Next
    </button>
  </div>
</template>
