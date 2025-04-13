<template>
<input type="text" v-model="page.title">
  Temps écoulé : {{ time }}
  <button @click="reset">reset</button>
</template>

<script setup>
import {ref, watch, watchEffect} from "vue";
import {useTimer} from "@/composable/useTimer.js";

const {time, reset} = useTimer()
const name = ref('')
const page = ref({
  title: ''
})

// watch(() => page.value.title, (newValue, oldValue) => {
//   document.title = newValue
// }, { immediate: true })

watchEffect(() => {
  document.title = page.value.title
})

</script>