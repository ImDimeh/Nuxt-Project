<script setup lang="ts">


const page = ref(1)
const Pagesize = ref(10)
const filters = ref([])
let filteredData = ref([])

function addToFilters(param) {
  if (!filters.value.includes(param)) {
    filters.value.push(param)
    console.log("FILTER VALUE", filters.value)
    console.log(data)
  } else {
    filters.value = filters.value.filter(e => e !== param)
    console.log("FILTER VALUE", filters.value)
    console.log(data)
  }
}



const { find } = useStrapi()
// Nom de la collection
// Nom de la collection
const { data, pending: bosesPending, error } = useAsyncData('boses', () => {
  return find('boses', {
    populate: '*',
    pagination: {
      page: page.value,
      pageSize: 3
    },

  })
}, {
  watch: [page, Pagesize, filters]
})







const { data: location, pending: locationPending, refresh } = useAsyncData('locations', () => {
  return find('locations', {
    populate: '*',
    pagination: {
      page: page.value,
      pageSize: Pagesize.value
    },


  })


}, {
  watch: [page, Pagesize, filters],
})



console.log(location)

</script>
<template>
  <div class="elden-ring-theme">
    <h1 class="title">Bienvenue dans l'univers d'Elden Ring</h1>

    <template v-if="bosesPending">
      <span>Loading...</span>
    </template>
    <template v-else>


      <h2>Location</h2>
      <div class="location-list">
        <div v-for="e in location?.data" class="location-item" :key="e.slug">
          <button class="location-button" @click="addToFilters(e.location)">{{ e.location }}</button>

          <a> {{ location.data.location }}</a>

        </div>

      </div>


      <h2> BOSS filtré</h2>
      <div class="boss-list">
        <div v-for="e in data?.data">
          <div class="card" v-if="filters.includes(e.Bosse_location.location)">

            <a class="boss-name" >{{ e.name }} </a>
            <a class="boss-surname"> {{ e.Surname }} </a>


            <img  class="bos-image" :src="e.image.formats.thumbnail.url" />

          </div>
        </div>

      </div>








      <h2>Les boses</h2>
      <div class="CardList">
        <div v-for="e in data?.data">
          <div class="card">

            <a>{{ e.name }} </a>
            <a> {{ e.Surname }} </a>


            <img :src="e.image.formats.thumbnail.url" />

          </div>
        </div>


      </div>
      <UPagination class="pagination" v-model="page" :page-count="data.meta.pagination.pageCount" :total="data.meta.pagination.total" />


    </template>

  </div>
</template>

<style scoped>

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  background-color: #D8CC78;

}
body{
  height: 100vh;
}
.pagination{
  display: flex;
  justify-content: center;
  margin-top: 20px;
  margin-bottom: 100px;

}
.elden-ring-theme {
  font-size: 24px;
    font-weight: bold;
    font-family: 'Arial', sans-serif;
    /* Utilisation d'une police similaire */
    color: #ef8b09;
    /* Couleur jaune dorée caractéristique d'Elden Ring */
    text-align: center;
    /* Centrer le titre */
    margin-top: 20px;
    /* Espace au-dessus du titre */
}

.title {
  text-align: center;
}

.location-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.location-item {
  margin: 10px;
}

.location-button {
  padding: 10px 20px;
  background-color: #6c757d;
  border: none;
  border-radius: 5px;
  color: #000000;
  cursor: pointer;
}

.boss-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.boss-card {
  background-color: #343a40;
  border-radius: 5px;
  margin: 10px;
  padding: 15px;
  width: 300px;
}

.boss-name,
.boss-surname {
  font-size: 18px;
  font-weight: bold;
  color: #000000;
  text-decoration: none;
}

.boss-image {
  width: 100%;
  border-radius: 5px;
  margin-top: 10px;
}

/* Pagination */
.pagination {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.page-item {
  margin: 0 5px;
}

.page-link {
  padding: 5px 10px;
  background-color: #6c757d;
  color: #000000;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.page-link:hover {
  background-color: #495057;
}

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
  background-color: #343a40;
}

.card a {
  background-color: transparent;
  font-size: 18px;
  font-weight: bold;
  color: #D8CC78;
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
h2{
  text-align: center;
  margin-top: 20px;
}

</style>