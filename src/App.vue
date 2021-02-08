<template>
<div>
  <nav class="navbar">
        <div class="container navbar-content">
            <h1>Where in the world?</h1>
        </div>
    </nav>
    
    <header class="container filter-content my-2">
        <form class="form-search" id="formulario">
           <i class="fas fa-search"></i>
        <input type="text" v-model="search" placeholder="Search for a country">
        
        </form>
        <div class="dropdownDiv" >
        <a 
          class="dropdownBtn" 
          v-if="!showAllRegion" 
          v-on:click="showFilter = !showFilter">Filter by Region 
        </a>

        <a 
          class="dropdownBtn" 
          v-else
          v-on:click="showFilter = !showFilter">
            {{ region }} 
        </a>
        <ul v-if="showFilter" class="dropdownUL">
          <li>
            <label for="radioAfrica">Africa
            <input 
              id="radioAfrica" 
              class="dropdownInput" 
              type="radio" 
              name="africa" 
              value="Africa" 
              v-model="region" 
              v-on:click="handleFilterClick"
            />
            </label>
          </li>
          <li>
            <input 
              id="radioAmerica" 
              class="dropdownInput" 
              type="radio" 
              name="america" 
              value="America" 
              v-model="region" 
              v-on:click="handleFilterClick"
            />
            <label for="radioAmerica">America</label>
          </li>
          <li>
            <input 
              id="radioAsia" 
              class="dropdownInput" 
              type="radio" 
              name="asia" 
              value="Asia" 
              v-model="region" 
              v-on:click="handleFilterClick"
            />
            <label for="radioAsia">Asia</label>
          </li>
          <li>
            <input 
              id="radioEurope" 
              class="dropdownInput" 
              type="radio" 
              name="europe" 
              value="Europe" 
              v-model="region" 
              v-on:click="handleFilterClick"
            />
            <label for="radioEurope">Europe</label>
          </li>
          <li>
            <input 
              id="radioOceania" 
              class="dropdownInput" 
              type="radio" 
              name="oceania" 
              value="Oceania" 
              v-model="region" 
              v-on:click="handleFilterClick"
            />
            <label for="radioOceania">Oceania</label>
          </li>
          <li v-if="showAllRegion">
            <input 
              id="radioAll" 
              class="dropdownInput" 
              type="radio" 
              name="all" 
              value="All Regions" 
              v-model="region" 
              v-on:click="handleFilterClick"
            />
            <label for="radioAll">All Regions</label>
          </li>
        </ul>
      </div>
        
    </header>
    
    <div v-if="countryInfo" class="container grid">
      <div v-for="country in filteredCountries" class="card" v-bind:key="country.id">
        
          <img v-bind:src="country.flag" alt="Country Flag" class="img-fluid">
          <div class="card-content">
            <h1>{{ country.name }}</h1>
            <p><b>Population: </b>{{ country.population | formatNumbers }}</p>
            <p><b>Region: </b> {{ country.region }}</p>
            <p><b>Capital: </b> {{ country.capital }}</p>
          </div>
      </div>
    </div>
</div>
</template>

<script>

import axios from 'axios'

export default {
 data(){
   return {
     
      pending: false,
      error: null,
      countryInfo: null,
      search: '',
      showFilter: false,
      showAllRegion: false,
      region: '',
      darkMode: false,
   }
 },
 mounted(){
   this.getTodos();
 },
 methods:{
   getTodos(){
     axios.get('https://restcountries.eu/rest/v2/all').then(response =>{
       this.countryInfo= response.data;
     }).catch(e => console.log(e))
   },
   handleFilterClick () {
      setTimeout(() => {
        this.showFilter = !this.showFilter;
        this.showAllRegion = true;
      })
    }
 },
 computed: {
    filteredCountries: function () {
      return this.countryInfo.filter((country) => {
        if (this.region === '' || this.region === 'All Regions') {
          return country.name.toLowerCase().match(this.search.toLowerCase());
        } else if (this.search !== '') {
          return country.name.toLowerCase().match(this.search.toLowerCase());
        } else {
          return country.region.match(this.region);
        }
      })
    }
  },  
  filters: {
    formatNumbers (value) {
      return `${value.toLocaleString()}`
    }
  }
  
}
</script>

<style>
:root {
    --color-bg: #ffffff;
    --color-bg-secondary: #ffffff;
    --color-text: #000000;
    --box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
    --border-radius: 0.35rem;
}

html {
    box-sizing: border-box;
}

*, *:before, *:after {
box-sizing: inherit;
}

body {
    --color-bg: #ffffff;
    --color-bg-secondary: #ffffff;
    --color-text: #000000;
    font-family: 'Nunito Sans', sans-serif;
    color: var(--color-text);
    background-color: var(--color-bg);
}



.container {
    margin-left: auto;
    margin-right: auto;
    width: 80%;
}

.img-fluid {
    width: 100%;
}

/* Espaciados */
.my-2 {
    margin-top: 2rem;
    margin-bottom: 2rem;
}

/* Navbar */
.navbar {
    background-color: var(--color-bg-secondary);
    box-shadow: var(--box-shadow);
}

.navbar-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
}


/* Filtros */
.filter-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.form-search {
    box-shadow: var(--box-shadow);
    border-radius: var(--border-radius);
    background-color: var(--color-bg-secondary);
    padding: 1rem;
}

.form-search i {
    opacity: 0.5;
}

.form-search input {
    border: none;
    outline: none;
    margin-left: 1rem;
    background-color: var(--color-bg-secondary);
    color: var(--color-text);
}

/* cards */
.grid {
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: 2rem;
}

.card {
    overflow: hidden;
    background-color: var(--color-bg-secondary);
    border-radius: var(--border-radius);
    box-shadow: var(--box-shadow);
}

.card img {
    border-top-left-radius: var(--border-radius);
    border-top-right-radius: var(--border-radius);
}

.card-content {
    padding: 2rem;
}

.card-content h3 {
    margin-bottom: 2rem;
}

.linkTile {

  text-decoration: none;
  color: inherit;
}
 .dropdownDiv {
    margin-top: 2px;
    
  }
  .dropdownBtn {
  display: block;
  background: #fff;
  height: 50px;
  width: 260px;
  box-shadow: 1px 1px 7px 0px rgb(0, 0, 0, 0.1);
  padding-right: 30px;
  padding-left: 30px;
  border-radius: 3px;
  cursor: pointer;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.dropdownBtn::after {
  font-family: "Font Awesome 5 Free";
  font-weight: 900;
  content: "\f107";
}

.dropdownUL {
  padding-left: 0;
  text-align: left;
  background-color: #fff;
  margin-top: 3px;
  padding: 10px 0; 
  box-shadow: 1px 1px 7px 0px rgb(0, 0, 0, 0.1);
  border-radius: 3px;
  position: absolute;
  width: 220px;
}

.dropdownUL li {
  list-style: none;
  line-height: 2;
  cursor: pointer;
}

.dropdownUL li label {
  cursor: pointer;
  padding: 0 26px;
  display: block;
  width: 148px;
}

.dropdownUL li:hover {
  background-color: #f9f9f9;
}

.dropdownInput {
  height: 1px;
  clip: rect(1px, 1px, 1px, 1px);
  position: absolute;
}

input[type="radio"] {
  -webkit-appearance: radio;
}


@media (max-width: 1000px){
    .grid {
        grid-template-columns: repeat(3, minmax(0, 1fr));
    }
}

@media (max-width: 750px){
    .grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }
    .dropdownBtn {
        width: 200px;
    }
}

@media (max-width: 600px){
    .navbar-content {
        font-size: 0.6rem;
    }

    .filter-content {
        flex-direction: column;
        align-items: flex-start;
    }

    .form-search {
        width: 100%;
        margin-bottom: 1rem;
    }

    .form-search input{
        width: 85%;
    }
    
    .grid {
        grid-template-columns: repeat(1, minmax(0, 1fr));
    }
}

</style>
