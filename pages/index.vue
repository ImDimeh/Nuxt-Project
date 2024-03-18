<script setup lang="ts">

const { find } = useStrapi()
// Nom de la collection
const { data, pending, error } = await useAsyncData('boses', async () => {
  return await find('boses', {
    populate: '*',
  })

})
</script>
<template>
  <div>
    <h1>Bienvenue</h1>
    <pre>{{ data }}</pre>
    

  </div>
  <About/>
</template>

<style scoped>
</style>