
    <template>
  <div id="app" :class="typeof weathers.main != 'undefined' && weathers.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="searchBox">
        <input type="text" 
        class="searchBar" 
        placeholder="Search..."
        v-model="query"
        @keypress="fetchWeather" 
        />
      </div>

      <div class="weatherWrap" v-if="typeof weathers.main != 'undefined'">
        <div class="locationBox">
          <div class="location">{{ weathers.name }}, {{ weathers.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
      </div>
      

      <div class="weatherBox">
        <div class="temp">23<sup>o</sup>C</div>
        <div class="weatherStatus">cloudy</div>
      </div>
    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      api_key: 'd573a0ecb765039db0fe03c2cfd09aa0', 
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weathers: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == 'Enter') {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
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
