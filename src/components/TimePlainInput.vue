<template>
  <input
    :value="props.modelValue"
    type="text"
    @input="$emit('update:modelValue', $event.target.value)"
    v-maska:[options]
    data-maska="##:##:##"
  />
</template>

<script setup lang="ts">
import { vMaska } from "maska";
import moment from "moment";
import { ref } from "vue";
import { defineEmits, defineProps } from "vue";

const emit = defineEmits(["update:modelValue"]);
const props = defineProps({ modelValue: String });
const time = ref(moment("00:00:00", "HH:mm:ss").format("HH:mm:ss"));
let memoryVal = props.modelValue ? props.modelValue : "00:00:00";

const findFirstDifferenceIndex = (str1: string, str2: string) => {
  const minLength = Math.min(str1.length, str2.length);

  for (let i = 0; i < minLength; i++) {
    if (str1[i] !== str2[i]) {
      return i;
    }
  }

  if (str1.length !== str2.length) {
    return minLength;
  }

  return -1;
};

const options = ref({
  postProcess: (val: string) => {
    const clearVal = val.split(":").join("");
    const clearOffVal = memoryVal.split(":").join("");
    const differentIndex = findFirstDifferenceIndex(clearVal, clearOffVal);
    console.log(differentIndex);
    console.log(clearVal);
    console.log(clearOffVal);
    console.log(clearOffVal[differentIndex]);
    let valToCalculate = clearVal;
    if (clearVal.length < 6) {
      valToCalculate =
        clearVal.slice(0, differentIndex) + clearOffVal.slice(differentIndex);
    } else {
      valToCalculate =
        clearVal.slice(0, differentIndex + 1) +
        clearOffVal.slice(differentIndex + 1);
    }
    let HH = valToCalculate.slice(0, 2);
    const hour = moment(HH, "HH");
    if (!(hour.isValid() && HH != "24")) HH = "23";
    let MM = valToCalculate.slice(2, 4);
    const minute = moment(MM, "mm");
    if (!(minute.isValid() && HH != "60")) MM = "59";
    let SS = valToCalculate.slice(4, 6);
    const second = moment(SS, "ss");
    if (!(second.isValid() && HH != "60")) SS = "59";
    valToCalculate = HH + ":" + MM + ":" + SS;
    memoryVal = valToCalculate;

    return valToCalculate;
  },
});
</script>

<style lang="scss" scoped>
input {
  border-radius: 5px;
  border: 1px solid #aaaaaa;
  padding: 5px 5px;
  font-size: 16px;
  width: 7ch;
}
</style>
