<script setup lang="ts">
const {findOne} = useStrapi()
const route = useRoute()
const router = useRouter()



const { data, pending, error } = await useAsyncData('boses', async () => {
  return await findOne(`boses/${route.params.id}`)

})

</script>

<template>
  <h1>Oui</h1>
  <div> {{ data.data.name }}</div>
  <div> {{ data.data.Surname }}</div>
  <img :src="data.data.image.url" alt="image" />
 
  <pre> {{ data }}</pre>
</template>