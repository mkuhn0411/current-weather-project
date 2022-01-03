<template>
  <section>
    <header>
      <h1>What's the Weather Right Now?</h1>
      <div class="city-container">
        <p 
          :class="{active: activeCity === city.name}" 
          @click="setActiveCity(city.name, city.key)" 
          v-for="city in cities" 
          :key="city.key"
        >{{city.name}}</p>
      </div>
      <div v-if="activeCity" class="city-card">
        <city-card 
          v-if="cityData"
          :city-name="cityName" 
          :city-data="cityData"
        />
      </div>
      <div v-else>
        <p>Please select a city</p>
      </div>
    </header>
  </section>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      cities: [
        { name: 'Denver', key: 5419384 },
        { name: 'Hurley', key: 5121679 },
        { name: 'Berlin', key: 2950159 },
        { name: 'Calgary', key: 5913490 },
        { name: 'Doha', key: 290030 },
      ],
      activeCity: null,
      cityName: '',
      cityData: null,
      
    }
  },
  methods: {
    setActiveCity(city, key) {
      this.activeCity = city;
      const id = key;
      this.getWeatherData(this.activeCity, id);
    },
    getWeatherData(city, key) {
      fetch('https://api.openweathermap.org/data/2.5/weather?id=' + key + '&units=imperial&appid=6e433fd4f34c477e8ebec3208ff603ad')
      .then(function(resp) { return resp.json() })
      .then(data => {
        console.log(data);
        this.cityData = data;
        this.cityName = city;
      })
      .catch(function(error) {
        console.log(error);
      });
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.city-container {
  display: flex;
  align-items: center;
  justify-content: center;
  list-style-type: none;
}
.city-container > p {
  margin: 0 5px;
  background-color: #2596be;
  border-radius: 40px;
  color: #fff;
  padding: 5px 10px;
  cursor: pointer;
}
.city-container > p.active {
  background-color: #15546a;
}
.country {
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center
}
.US {
  background-image: url(https://img.freeflagicons.com/thumb/round_icon/united_states_of_america/united_states_of_america_640.png)
}
</style>