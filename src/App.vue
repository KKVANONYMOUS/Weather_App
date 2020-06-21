<template>
  <div id="app">
     <b-container fluid :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <div id="card">
      <b-card overlay v-bind:img-src="url" img-alt="Image" img-top tag="article" style="max-width: 22rem;" class="mb-2">

        <b-card-text>
          <b-form-input size="sm" class="mr-sm-2" placeholder="Search" v-model="locInput" @keypress="fetchWeather">
          </b-form-input>
          <br><br>
          <div id="content" v-if="typeof weather.main != 'undefined'">
            <div id="location">
              {{ weather.name }}, {{ weather.sys.country }}
            </div>
            <div id="date">
              {{CurrentDate()}}
            </div>
            <div id="box">
              <img v-bind:src='icon'>
              <br>
              {{ weather.weather[0].main }}

              <p>
                {{ Math.round(weather.main.temp) }}°C
              </p>
              <p id="extra">
                Humidity: {{weather.main.humidity}}%<br>
                Wind: {{weather.wind.speed}}km/h
              </p>
            </div>

          </div>
        </b-card-text>


      </b-card>
    </div>
  </b-container>
  </div>
</template>

<script>


export default {
  name: 'App',
   data() {
      return {
        icon: '',
        url: 'src/assets/Outdoor.jpg',
        apiKey: '49a6eadbbac4095d97833c7907440549',
        baseUrl: 'https://api.openweathermap.org/data/2.5/',
        locInput: '',
        weather: {}

      }
    },
    methods: {
      fetchWeather: function (e) {
        if (e.key == 'Enter') {
          fetch(`${this.baseUrl}weather?q=${this.locInput}&units=metric&APPID=${this.apiKey}`)
            .then(data => {
              if (data.status == '200') {
                return data.json();
              } else if (data.status == '404') {
                alert('Please give valid input')
              } else {
                alert('Some error occured')
              }

            }).then(this.setWeather);
        }
      },
      setWeather(res) {
        this.weather = res;
        var code = this.weather.weather[0].icon;
        this.icon = "http://openweathermap.org/img/wn/" + code + "@2x.png";
      },
      CurrentDate: function () {
        var d = new Date()
        var months = ['Januray', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September',
          'October', 'November', 'December'
        ];
        var days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
        var date = d.getUTCDate();
        var year = d.getUTCFullYear();
        var month = months[d.getUTCMonth()];
        var day = days[d.getUTCDay()];
        return day + ',' + date + ' ' + month + ' ' + year;
      }


    },
    created() {
      fetch(`${this.baseUrl}weather?q=Delhi&units=metric&APPID=${this.apiKey}`)
        .then(data => {
          if (data.status == '200') {
            return data.json();
          } else {
            alert('Some error occured')
          }

        }).then(this.setWeather);
    }
}
</script>

<style>
 @import url('https://fonts.googleapis.com/css2?family=Nunito:wght@600&display=swap');

  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Nunito', sans-serif;
  }

  #card {
    margin: 50px auto;
    max-width: 22rem;
  }

  .mr-sm-2 {
    margin-top: 40px;
    opacity: 0.7;
    border-top-right-radius: 10px;
    border-bottom-left-radius: 10px;
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    border-width: 0;
    transition: 0.5s;
    height: 41px;

  }

  .mr-sm-2:focus {
    border-top-left-radius: 10px;
    border-bottom-right-radius: 10px;
    border-top-right-radius: 0;
    border-bottom-left-radius: 0;
    outline: none;
    appearance: none;
    border: none;
  }

  #location {
    text-align: center;
    color: rgb(229, 234, 235);
    font-size: 30px;
    font-weight: bolder;
  }

  #date {
    text-align: center;
  }

  #extra {
    font-size: 15px;
    opacity: 1;
  }

  #box {
    border: 1px solid black;
    border-radius: 15px;
    border-width: 0;
    visibility: 0.5;
    font-size: 30px;
    color: rgba(0, 0, 0, 0.25);
    text-align: center;
    width: 200px;
    margin: 0 auto;
    background-color: rgba(255, 255, 255, 0.5);
  }

  p {
    opacity: 1;
    color: #000;
  }
</style>
