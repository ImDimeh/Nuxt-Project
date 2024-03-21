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
      <div class="CardList">
        <div v-for="e in data?.data">
          <div class="card" v-if="filters.includes(e.Bosse_location.location) ">

            <a>{{ e.name }} </a>
            <a> {{ e.Surname }} </a>


            <img :src="e.image.formats.thumbnail.url " />

          </div>
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
.CardList {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  /* Centers the cards horizontally */
  flex-direction: row;
}

.card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  width: 300px;
  /* Set your desired width */
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  margin: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  height: 300px;
}

.card a {
  font-size: 18px;
  font-weight: bold;
  color: #333;
  text-decoration: none;
}

.card img {
  width: 100%;
  height: auto;
  display: block;
  margin-top: 10px;
  border-radius: 5px;
}

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