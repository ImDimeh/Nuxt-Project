<script setup lang="ts">


const page = ref(1)
const Pagesize = ref(2)
const filters = ref([])

const { find } = useStrapi()
// Nom de la collection
const { data, pending: bosesPending, error } = useAsyncData('boses', () => {
  return find('boses', {
    populate: '*',
  })

})

const { data: location, pending: locationPending  , refresh} = useAsyncData('locations', () => {
  return find('locations', {
    populate: '*',
    pagination: {
      page: page.value,
      pageSize: Pagesize.value
    },
    filters: {
      location: {
        $in: filters.value
      }
    }

  }), {
    watch: [page, Pagesize, filters],
  }
    

})

refresh()


</script>
<template>
  <div>
    <h1>Bienvenue</h1>
   
    <template v-if="bosesPending">
        <span>Loading...</span>
    </template>
    <template v-else>
      
      <h2>Location</h2>
      <div class="LocattionList">
        <div v-for="e in location?.data" class="" :key="e.slug">
          <button>{{ e.location }}</button>
          
        </div>

      </div>
      <pre> {{ location }}</pre>



      <h2>Les boses</h2>
      <div v-for="e in data?.data" :key="e.slug">
        <a :href="`/boses/${e.slug}`"  :key="e.id"  class="underline">{{ e.name }}</a>
      </div>
     
    </template>
    
  </div>
</template>

<style scoped>
/* <UPagination v-model="page"  :page-count="data?.meta.total"  :total="data?.meta.total" /> */
button {
  margin: 10px;
  padding: 10px;
  background-color: #f5f5f5;
  border: 1px solid #000;
  border-radius: 5px;
  color: #000;
}
.LocattionList {
  padding: 10px;
  
  
  border: 1px solid #000;
  border-radius: 5px;
  color: #000;
  display: flex;
  flex-direction: row;
  width: auto;
} 
</style>