<template>
  <div class="country-detail" v-if="country">
    <h1>{{ country?.name?.common }}</h1>
    <div class="info">
      <img :src="country.flags.png" alt="Country flag" class="country-flag" />
      <div class="country-info">
        <p><strong>Native Name:</strong> {{ country?.name?.common }}</p>
        <p><strong>Capital:</strong> {{ country.capital[0] }}</p>
        <p><strong>Population:</strong> {{ country.population }}</p>
        <p><strong>Region:</strong> {{ country.region }}</p>
        <p><strong>Sub-region:</strong> {{ country.subregion }}</p>
        <p><strong>Area:</strong> {{ country.area }} km²</p>
        <p>
          <strong>Languages:</strong>
          {{ country.languages.eng }}
        </p>
        <p><strong>Timezones:</strong> {{ country.timezones.join(", ") }}</p>
      </div>
    </div>
    <div class="neighbour-countries-container" v-if="borderFlags.length">
      <h2>Neighbour Countries</h2>
      <div class="neighbour-countries">
        <img
          v-for="flag in borderFlags"
          :key="flag"
          :src="flag"
          class="neighbour-flag"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      country: null,
      borderFlags: [],
    };
  },
  async created() {
    const countryName = this.$route.params.countryName;
    try {
      const response = await axios.get(
        `https://restcountries.com/v3.1/name/${countryName}?fullText=true`
      );

      this.country = response.data[0];

      try {
        const borderCodes = response.data[0].borders;
        const codes = borderCodes.join(",");
        const countryBorder = await axios.get(
          `https://restcountries.com/v3.1/alpha?codes=${codes}`
        );
        const border = countryBorder.data.map((e) => e?.flags.png);
        this.borderFlags = border;
        return border ? border.flags.png : "";
      } catch (error) {
        console.error(
          "An error occurred while fetching the country details:",
          error
        );
      }
    } catch (error) {
      console.error(
        "An error occurred while fetching the country details:",
        error
      );
    }
  },
  methods: {
    async getBorderFlag(borderCodes) {
      try {
        const codes = borderCodes.join(",");
        const response = await axios.get(
          `https://restcountries.com/v3.1/alpha?codes=${codes}`
        );
        const border = response.data.map((e) => e?.flags.png);
        this.borderFlags = border;
        return border ? border.flags.png : "";
      } catch (error) {
        console.error(
          "An error occurred while fetching the country details:",
          error
        );
      }
    },
  },
};
</script>

<style>
.info {
  display: flex;
}
.country-detail {
  width: 700px;
  margin: 0 auto;
}
.country-flag {
  width: 300px;
  height: 180px;
  margin-bottom: 20px;
}
.neighbour-countries-container {
  border: 1px solid;
  padding: 30px;
}
.neighbour-countries {
  display: flex;
  flex-wrap: wrap;
}
.neighbour-flag {
  width: 180px;
  height: 100px;
  margin: 5px;
  padding: 10px;
}
</style>
