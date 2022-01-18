<template>
  <div class="app" >
    <header>
      <h1>Buscador de<strong>Anime</strong></h1>

      <form class="search-box" @submit.prevent="HeadleSearch" >
      <input type="search" class="search-field"
      placeholder="Procure por um anime..."
      required
      v-model="search_query">
    </form>
    </header>

    <main>
      <div class="cards" >
        <Card v-for="anime in animelist" :key="anime.mal_id" :anime="anime" />
      </div>
    </main>
    
  </div>
</template>


<script>
import { ref } from 'vue'
import Card from './components/Card.vue'

export default {
  setup(){
    const search_query = ref("")
    const animelist = ref([])

    const HeadleSearch = async () => {
      animelist.value = await fetch(`https://api.jikan.moe/v3/search/anime?q=${search_query.value}`)
      .then(res => res.json())
      .then(data => data.results)

      search_query.value = ""
    }

    return{
    search_query,
    animelist,
    HeadleSearch
    }
  },
  components:{
    Card,
  }
  
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;

  font-family: 'Fira Sans', sans-serif;
}
a{
  text-decoration: none;
}

header{
  padding-top: 50px;
  padding-bottom: 50px;
}

h1{
  color: #888;
  font-size: 42px;
  font-weight: 400;
  text-align: center;
  text-transform: uppercase;
  margin-bottom: 30px;
}

h1:hover{
  color: #313131;
}

strong{
  color: #313131;
}

.search-box{
  display: flex;
  justify-content: center;
  padding-left: 30px;
  padding-right: 30px;
}

.search-field{
  appearance: none;
  background: none;
  border: none;
  outline: none;

  background-color: #f3f3f3;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);

  display: block;
  width: 100%;
  max-width: 600px;
  padding: 15px;
  border-radius: 8px;

  color: #313131;

  font-size: 20px;

  transition: 0.4s;
}

.search-field::placeholder{
  color: #AAA;
}

.search-field:focus, .search-field:focus:valid{
  color: #fff;
  background-color: #313131;
  box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
}

main{
  max-width: 1200px;
  margin: 0 auto;
  padding-left: 30px;
  padding-right: 30px;
}

.cards{
  display: flex;
  flex-wrap: wrap;
  margin: 0 -8px;
}
</style>
