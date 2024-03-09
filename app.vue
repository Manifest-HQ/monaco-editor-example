<template>
  <div style="background-color: #202020; padding: 24px 16px">
    <MonacoEditor
      v-model="value"
      lang="javascript"
      :options="{
        theme: 'vs-dark'
      }"
      style="height: 100vh;" />
  </div>
</template>

<script setup>
const value = ref(`const a = 1
const b = 2
const c = 3

function sum() {
  return a + b + c
}

console.log(sum())`)
</script>

<style lang="sass">
html, body
  margin: 0
</style>