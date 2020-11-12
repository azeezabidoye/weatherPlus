  
    <template>
  <div id="app" :class="typeof weathers.current != 'undefined' && weathers.current.temperature > 16 ? 'warm' : ''">
    <main>
      <div class="searchBox">
        <input type="text" 
        class="searchBar" 
        placeholder="Search..."
        v-model="query"
        @keypress="fetchWeather" 
        />
      </div>

      <div class="weatherWrap" v-if="typeof weathers.current != 'undefined'">
        <div class="locationBox">
          <div class="location">{{ weathers.location.name }}, {{ weathers.location.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
      </div>
      

      <div class="weatherBox" v-if="typeof weathers.current != 'undefined'">
        <div class="temp">{{ weathers.current.temperature }}<sup>o</sup>C</div>
        <div class="weatherStatus">{{ weathers.current.weather_descriptions[0] }}</div>
      </div>
    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      api_key: '712487c88ee4fb5706ae8cce8e09cd17', 
      url_base: 'http://api.weatherstack.com/',
      query: '',
      weathers: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == 'Enter') {
        fetch(`${this.url_base}current?access_key=${this.api_key}&query=${this.query}&units=m`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weathers = results;
    },
    dateBuilder() {
      let d = new Date();
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

      let date = d.getDate();
      let day = days[d.getDay()];
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;

    }
  }
}
</script>
