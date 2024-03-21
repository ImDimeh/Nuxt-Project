<script setup lang="ts">


const page = ref(1)
const Pagesize = ref(10)
const filters = ref([])
let filteredData = ref([])

function addToFilters(param) {
  if (!filters.value.includes(param)) {
    filters.value.push(param)
    console.log("FILTER VALUE" ,filters.value)
    console.log(data)
  } else {
    filters.value = filters.value.filter(e => e !== param)
    console.log("FILTER VALUE" ,filters.value)
    console.log(data)
  }
}


  
const { find } = useStrapi()
// Nom de la collection
// Nom de la collection
const { data, pending: bosesPending, error } = useAsyncData('boses', () => {
  return find('boses', {
    populate: '*',
    
  })
}, {
  watch: [page, Pagesize, filters]
})







const { data: location, pending: locationPending  , refresh} = useAsyncData('locations', () => {
  return find('locations', {
    populate: '*',
    pagination: {
      page: page.value,
      pageSize: Pagesize.value
    },
    

  })
    

},{
    watch: [page, Pagesize, filters],
})



console.log(location)

</script>
<template>
  <div>
    <h1>Bienvenue</h1>

    <template v-if="bosesPending">
      <span>Loading...</span>
    </template>
    <template v-else>
      {{ filteredData }}

      <h2>Location</h2>
      <div class="LocattionList">
        <div v-for="e in location?.data" class="" :key="e.slug">
          <button @click="addToFilters(e.location)">{{ e.location }}</button>

          <a> {{ location.data.location }}</a>

        </div>

      </div>
      <div> {{ filters }}</div>

      <h1> BOSS filtré</h1>

      <div v-for="e in data?.data">
        <div v-if="filters.includes(e.Bosse_location.location) ">

          <a>{{ e.name }} {{ e.Surname }}</a>
          <a> {{ e.image.formats.small }}</a>

          <img
            :src="e.image.formats.small.url || `https://image.api.playstation.com/vulcan/ap/rnd/202107/1612/Y5RHNmzAtc6sRYwZlYiKHAxN.png`"  alt="
            image" />

        </div>
      </div>








      <h2>Les boses</h2>
      <div v-for="e in data?.data" :key="e.slug">
        <a :href="`/boses/${e.slug}`" :key="e.id" class="underline">{{ e.name }}</a>
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
  /* color: #000; */
  display: flex;
  flex-direction: row;
  width: auto;
} 
</style>