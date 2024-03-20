<template>
  <div style="background-color: #202020; padding: 24px 16px">
    <MonacoEditor
      v-model="value"
      lang="prisma"
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

onMounted(() => {
  const monaco = useMonaco()

  monaco.languages.register({ id: 'prisma' })
  monaco.languages.setMonarchTokensProvider('prisma', {
    tokenizer: {
        root: [
            // Keywords
            [/\b(?:model|datasource|generator|type|enum)\b/, 'keyword'],
            // Types
            [/\b(?:String|Int|Boolean|DateTime|Float)\b/, 'type'],
            // Comments
            [/#.*$/, 'comment'], // hash style comments
            [/\/\/.*$/, 'comment'], // double-slash comments
            // Strings
            [/"(?:[^"\\]|\\.)*"/, 'string'], // double quoted strings
            [/'(?:[^'\\]|\\.)*'/, 'string'], // single quoted strings
            // Identifiers
            [/\b[a-zA-Z_]\w*\b/, 'identifier'],
            // Brackets and Operators
            [/[{}()\[\]]/, '@brackets'],
            [/[,]/, 'delimiter'],
            [/=/, 'operator'],
            // For simplicity, let's remove the problematic pattern and not focus on < and > symbols for now.
            // If needed, specific patterns for operators or specific symbols can be added separately.
        ],
    }
  })
})
</script>

<style lang="sass">
html, body
  margin: 0
</style>