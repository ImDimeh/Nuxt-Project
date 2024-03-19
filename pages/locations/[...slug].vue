<script setup lang="ts">
const { findOne } = useStrapi()
const route = useRoute()
const router = useRouter()
</script>

<template>
  <h1>Oui</h1>
  <div>{{ data.data.name }}</div>
  <div>{{ data.data.location }}</div>
  <pre>{{ data }}</pre>
</template>

<style scoped>
h1 {
  color: blue;
}

div {
  font-size: 16px;
  margin-bottom: 10px;
}

pre {
  background-color: #f5f5f5;
  padding: 10px;
}
</style>
