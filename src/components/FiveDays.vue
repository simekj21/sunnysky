<template>
    <div class="mx-1 flex flex-col gap-y-1 items-stretch bg-slate-500">
        <div class="flex flex-col gap-y-2 sticky top-0 bg-slate-500">
            <div class="flex flex-row justify-between font-bold  text-4xl text-red-600 p-4 rounded-md bg-slate-600">
                <svg 
                    class="cursor-pointer self-center" 
                    style="width:50px;height:50px" 
                    viewBox="0 0 30 30"
                    @click="toggleParams()" 
                >
                    <path fill="red" d="M15,5H17V3H15M15,13H17V11H15M15,21H17V19H15M11,5H13V3H11M19,5H21V3H19M11,9H13V7H11M19,9H21V7H19M19,21H21V19H19M19,13H21V11H19M19,17H21V15H19M11,13H13V11H11M3,5H5V3H3M3,9H5V7H3M3,13H5V11H3M3,17H5V15H3M3,21H5V19H3M11,21H13V19H11M11,17H13V15H11M7,21H9V19H7M7,13H9V11H7M7,5H9V3H7V5Z" />
                </svg>
                <h1>Sunny Sky</h1>
                <h1 class="text-sm self-center">{{ weatherData.city_name }}</h1>
            </div>
            
            <div class="
                flex flex-row justify-start            
                font-bold  text-4xl text-pink-600
                p-4 rounded-md bg-slate-600
            ">
                <svg 
                    v-for="(param, index) in params" :key="index"
                    class="cursor-pointer" style="width:50px;height:50px" viewBox="0 0 30 30"
                    @click="param.show = !param.show" 
                >
                    <path :fill="param.show ? 'deepskyblue' : 'gray'" :d="param.svg" />
                </svg>
            </div>
        </div>

        <div class="overflow-hidden">
            <div class="flex flex-col gap-1 items-stretch">
                <div 
                    v-for="(day, index) in weatherData.data" :key="index"
                    class="
                        p-4 flex flex-row items-center flex-wrap gap-1
                        rounded-md bg-slate-600
                    "
                >
                    <div class="flex flex-col items-center">
                        <h1 class="mb-2 p-1 text-xl text-pink-400">
                            {{ getDay(index) }} 
                        </h1>    
                        <h2 class="mb-2 p-1 text-sm text-white bg-slate-500">
                            {{ day.datetime }}
                        </h2>    
                        <h2 class="mb-2 p-1 text-xs text-white bg-slate-500">
                            {{ getSunset(day.sunset_ts) }}
                        </h2>    
                    </div>

                    <img 
                        :src="getIconUrl(index)"
                        width="70"
                    >

                    <div class="flex flex-row justify-center gap-x-3">
                        <h1 class="mt-3 p-2 text-sm rounded bg-blue-400 text-white font-bold">
                            {{ day.min_temp }}&#176;
                        </h1>    
                        <h1 class="mt-3 p-2 outline-double outline-offset-1 outline-blue-400 rounded text-white font-bold">
                            {{ day.temp }}&#176; 
                        </h1>    
                        <h1 class="mt-3 p-2 text-sm rounded bg-red-400 text-white font-bold">
                            {{ day.max_temp }}&#176;
                        </h1>    
                    </div>

                    <div 
                        v-for="(param, index) in params" :key="index"
                        class="flex flex-row justify-center gap-x-1"
                    >
                        <h1 v-show="param.show" class="bg-slate-400 cursor-pointer mt-3 p-2 text-xs rounded-full text-white">
                            {{ day[param.key] }}{{ param.unit }}
                        </h1>  
                    </div>
                </div>
            </div>
        </div>

        <h1 class="pb-4 text-sm self-center text-gray-50">
            {{ weatherData.country_code }} - 
            {{ weatherData.timezone }} - 
            {{ weatherData.city_name }} - 
            lat {{ weatherData.lat }} - 
            lon {{ weatherData.lon }}
        </h1>
    </div>
</template>

<script>
export default {
  name: 'FiveDays',
  props: {
  },
  data() {
      return {
        weatherData: {},
        iconUrl: 'https://www.weatherbit.io/static/img/icons/',
        daysInWeek: ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'],
        showWind: false,
        showPres: false,
        showPop: false,
        showRh: false,
        showClouds: false,
        showVis: false,
        showMoon: false,

        params: [
            {
                key: 'pres',
                show: false,
                unit: '',
                svg: 'M12,2A10,10 0 0,0 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12A10,10 0 0,0 12,2M12,4A8,8 0 0,1 20,12C20,14.4 19,16.5 17.3,18C15.9,16.7 14,16 12,16C10,16 8.2,16.7 6.7,18C5,16.5 4,14.4 4,12A8,8 0 0,1 12,4M14,5.89C13.62,5.9 13.26,6.15 13.1,6.54L11.81,9.77L11.71,10C11,10.13 10.41,10.6 10.14,11.26C9.73,12.29 10.23,13.45 11.26,13.86C12.29,14.27 13.45,13.77 13.86,12.74C14.12,12.08 14,11.32 13.57,10.76L13.67,10.5L14.96,7.29L14.97,7.26C15.17,6.75 14.92,6.17 14.41,5.96C14.28,5.91 14.15,5.89 14,5.89M10,6A1,1 0 0,0 9,7A1,1 0 0,0 10,8A1,1 0 0,0 11,7A1,1 0 0,0 10,6M7,9A1,1 0 0,0 6,10A1,1 0 0,0 7,11A1,1 0 0,0 8,10A1,1 0 0,0 7,9M17,9A1,1 0 0,0 16,10A1,1 0 0,0 17,11A1,1 0 0,0 18,10A1,1 0 0,0 17,9Z',
            },
            {
                key: 'wind_spd',
                show: false,
                unit: 'm/s',
                svg: 'M7 5V13L22 11V7L7 5M10 6.91L13 7.31V10.69L10 11.09V6.91M16 7.71L19 8.11V9.89L16 10.29V7.71M5 10V11H6V12H5V21H3V4C3 3.45 3.45 3 4 3S5 3.45 5 4V6H6V7H5V10Z',
            },
            {
                key: 'clouds',
                show: false,
                unit: '%',
                svg: 'M19,18H6A4,4 0 0,1 2,14A4,4 0 0,1 6,10H6.71C7.37,7.69 9.5,6 12,6A5.5,5.5 0 0,1 17.5,11.5V12H19A3,3 0 0,1 22,15A3,3 0 0,1 19,18M19.35,10.03C18.67,6.59 15.64,4 12,4C9.11,4 6.6,5.64 5.35,8.03C2.34,8.36 0,10.9 0,14A6,6 0 0,0 6,20H19A5,5 0 0,0 24,15C24,12.36 21.95,10.22 19.35,10.03Z',
            },
            {
                key: 'pop',
                show: false,
                unit: '%',
                svg: 'M9,12C9.53,12.14 9.85,12.69 9.71,13.22L8.41,18.05C8.27,18.59 7.72,18.9 7.19,18.76C6.65,18.62 6.34,18.07 6.5,17.54L7.78,12.71C7.92,12.17 8.47,11.86 9,12M13,12C13.53,12.14 13.85,12.69 13.71,13.22L11.64,20.95C11.5,21.5 10.95,21.8 10.41,21.66C9.88,21.5 9.56,20.97 9.7,20.43L11.78,12.71C11.92,12.17 12.47,11.86 13,12M17,12C17.53,12.14 17.85,12.69 17.71,13.22L16.41,18.05C16.27,18.59 15.72,18.9 15.19,18.76C14.65,18.62 14.34,18.07 14.5,17.54L15.78,12.71C15.92,12.17 16.47,11.86 17,12M17,10V9A5,5 0 0,0 12,4C9.5,4 7.45,5.82 7.06,8.19C6.73,8.07 6.37,8 6,8A3,3 0 0,0 3,11C3,12.11 3.6,13.08 4.5,13.6V13.59C5,13.87 5.14,14.5 4.87,14.96C4.59,15.43 4,15.6 3.5,15.32V15.33C2,14.47 1,12.85 1,11A5,5 0 0,1 6,6C7,3.65 9.3,2 12,2C15.43,2 18.24,4.66 18.5,8.03L19,8A4,4 0 0,1 23,12C23,13.5 22.2,14.77 21,15.46V15.46C20.5,15.73 19.91,15.57 19.63,15.09C19.36,14.61 19.5,14 20,13.72V13.73C20.6,13.39 21,12.74 21,12A2,2 0 0,0 19,10H17Z',
            },
            {
                key: 'rh',
                show: false,
                unit: '',
                svg: 'M20 14H22V16H20C18.62 16 17.26 15.65 16 15C13.5 16.3 10.5 16.3 8 15C6.74 15.65 5.37 16 4 16H2V14H4C5.39 14 6.78 13.53 8 12.67C10.44 14.38 13.56 14.38 16 12.67C17.22 13.53 18.61 14 20 14M20 20H22V22H20C18.62 22 17.26 21.65 16 21C13.5 22.3 10.5 22.3 8 21C6.74 21.65 5.37 22 4 22H2V20H4C5.39 20 6.78 19.53 8 18.67C10.44 20.38 13.56 20.38 16 18.67C17.22 19.53 18.61 20 20 20M7 2L3 6H6V11H8V6H11M17 2L13 6H16V11H18V6H21',
            },
            {
                key: 'vis',
                show: false,
                unit: 'km',
                svg: 'M21.9,8.9L20.2,9.9L16.2,3L17.9,2L21.9,8.9M9.8,7.9L12.8,13.1L18.9,9.6L15.9,4.4L9.8,7.9M11.4,12.7L9.4,9.2L5.1,11.7L7.1,15.2L11.4,12.7M2.1,14.6L3.1,16.3L5.7,14.8L4.7,13.1L2.1,14.6M12.1,14L11.8,13.6L7.5,16.1L7.8,16.5C8,16.8 8.3,17.1 8.6,17.3L7,22H9L10.4,17.7H10.5L12,22H14L12.1,16.4C12.6,15.7 12.6,14.8 12.1,14Z',
            },
            {
                key: 'moon_phase_lunation',
                show: false,
                unit: '',
                svg: 'M17.75,4.09L15.22,6.03L16.13,9.09L13.5,7.28L10.87,9.09L11.78,6.03L9.25,4.09L12.44,4L13.5,1L14.56,4L17.75,4.09M21.25,11L19.61,12.25L20.2,14.23L18.5,13.06L16.8,14.23L17.39,12.25L15.75,11L17.81,10.95L18.5,9L19.19,10.95L21.25,11M18.97,15.95C19.8,15.87 20.69,17.05 20.16,17.8C19.84,18.25 19.5,18.67 19.08,19.07C15.17,23 8.84,23 4.94,19.07C1.03,15.17 1.03,8.83 4.94,4.93C5.34,4.53 5.76,4.17 6.21,3.85C6.96,3.32 8.14,4.21 8.06,5.04C7.79,7.9 8.75,10.87 10.95,13.06C13.14,15.26 16.1,16.22 18.97,15.95M17.33,17.97C14.5,17.81 11.7,16.64 9.53,14.5C7.36,12.31 6.2,9.5 6.04,6.68C3.23,9.82 3.34,14.64 6.35,17.66C9.37,20.67 14.19,20.78 17.33,17.97Z',
            },
        ],

        windyClass: {
            windyBackActive: false,
        },

        temperaturesAll: {},
        temperaturesFive: [],
        tempsFive: [],
        daysFive: [],
        days: ['Monday','Tuesday','Wendsday','Thursday','Friday','Saturday','Sunday'],
        heightDay: 70,
        heightDayClass: '',
        heightDayClasses: [],
      }
  },
  methods: {
      toggleParams() {
          this.params.forEach( param => { param.show = !param.show  })
      },
      getIconUrl(index) {
          let iconAddress = 'https://www.weatherbit.io/static/img/icons/' + this.weatherData.data[index].weather.icon + '.png'
          return iconAddress
      },
      getDay(index) {
          return this.daysInWeek[new Date(this.weatherData.data[index].valid_date).getDay()]
      },
      getSunset(sunset) {
          return new Date(sunset * 1000).toLocaleTimeString()
      },
      toggleWind() {
        this.showWind = !this.showWind
      },
  },
  computed: {
      urlOfIcon: function () {
          return this.message.split('').reverse().join('')
        },
  },
  created() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(position => {
        console.log("Latitude: ", position.coords.latitude, 
        "Longitude: ", position.coords.longitude)

        let weatherURL = 
            `https://api.weatherbit.io/v2.0/forecast/daily?lat=${position.coords.latitude}&lon=${position.coords.longitude}&key=6c5d31259b284ea3857143ed606db594`
  
        console.log('url', weatherURL)

        fetch(weatherURL)
            .then(response => response.json())
            .then(response => {
                    this.weatherData = response
                        console.log('DATA:', this.weatherData)
                        localStorage.setItem('weather', JSON.stringify(this.weatherData))
                })
            .catch(err => console.error(err))
    })  } else { 
        alert("Geolocation is not supported by this browser.")
    }
  },
}
</script>
