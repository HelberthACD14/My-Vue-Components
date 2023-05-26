<template>
  <h2>Input plain time</h2>
  <time-plain-input v-model="test"></time-plain-input>
  <h4>{{ test }}</h4>
</template>

<script setup lang="ts">
import TimePlainInput from "@/components/TimePlainInput.vue";
import { ref } from "vue";

const test = ref("14:23:34");
</script>
<style lang="sss" scoped></style>
