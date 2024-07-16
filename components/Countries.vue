<template>
  <div class="container">
    <h1 class="header">Countries</h1>
    <div class="search-container">
      <input type="text" v-model="search" placeholder="Search countries" />
      <i class="fas fa-search search-icon"></i>
    </div>
    <div
      v-for="country in filteredCountries"
      :key="country.name"
      class="country-card"
    >
      <img :src="country.flag" alt="Country flag" class="country-flag" />
      <div class="country-info">
        <h2>{{ country.name }}</h2>
        <p class="country-info-text">
          Currency: {{ country?.currencies && country?.currencies[0].name }}
        </p>
        <p class="country-info-text">
          Current date and time: {{ currentDateTime }}
        </p>
        <div class="country-info-buttons">
          <button @click="showMap(country.name)">Show Map</button>
          <button @click="showDetail(country.name)">Detail</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import "@fortawesome/fontawesome-free/css/all.css";
import "@fortawesome/fontawesome-free/js/all.js";

export default {
  data() {
    return {
      search: "",
      countries: [],
      currentDateTime: new Date().toLocaleString(),
    };
  },
  async created() {
    try {
      const response = await axios.get("https://restcountries.com/v2/all");
      this.countries = response.data;
    } catch (error) {
      console.error("An error occurred while fetching the countries:", error);
    }
  },
  computed: {
    filteredCountries() {
      if (this.search.toLowerCase()) {
        return this.countries.filter((country) =>
          country.name.toLowerCase().includes(this.search.toLowerCase())
        );
      } else {
        return this.countries.splice(0, 2);
      }
    },
  },
  methods: {
    showMap(name) {
      const mapUrl = `https://www.google.com/maps/search/${name}`;
      window.open(mapUrl, "_blank");
    },
    showDetail(code) {
      this.$router.push(`/country/${code}`);
    },
  },
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 700px;
}
.header {
  align-self: flex-start;
  margin-left: 10%;
}
.search-container {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  width: 83%;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.search-container input {
  width: 100%;
  padding: 14px;
  border: none;
}
.search-icon {
  height: 26px;
  color: #aaa;
  padding: 10px;
}
.country-card {
  display: flex;
  align-items: center;
  border: 1px solid #ccc;
  margin-bottom: 10px;
  padding: 10px;
  width: 80%;
}
.country-flag {
  width: 200px;
  height: 120px;
  margin-right: 20px;
}
.country-info {
  flex-grow: 1;
}
.country-info h2 {
  margin-top: 2px;
  margin-bottom: 5px;
}
.country-info p {
  margin: 5px 0px;
}
.country-info-buttons {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
button {
  margin-right: 10px;
  background-color: #fff;
  border: 2px solid #007bff;
  color: #007bff;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 4px;
  width: 150px;
}
button:hover {
  background-color: #007bff;
  color: #fff;
}
</style>
