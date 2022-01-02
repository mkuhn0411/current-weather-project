<template>
    <div class="card-container">
        <div class="row row-1">
            <p class="city"> {{ cityName }} <img class="country" :src="countries[cityData.sys.country]"> </p>
            <div 
                class="image"
                :class="weatherClass"
            >&nbsp;</div>
        </div>
        <div class="row row-2">
            <p><strong> Current Forecast:</strong> {{ cityData.weather[0].description }} </p>
            <p><strong> Current Temperature:</strong> {{ Math.round(cityData.main.temp) }}° </p>
            <p><strong> Feels Like:</strong> {{ Math.round(cityData.main.feels_like) }}° </p>
            <div class="set-container">
                <div class="sunrise-container set-sub-container">
                    <img class="sunrise set-icon" src="https://s3.amazonaws.com/jebbit-assets/images/GyIlxwsk/business-images/04MIVE9CSVSGQZt49knl_sun.png">
                    <p class="time">{{ handleSet(cityData.sys.sunrise) }} AM</p>
                </div>
                <div class="sunset-container set-sub-container">
                    <img class="sunset set-icon" src="https://s3.amazonaws.com/jebbit-assets/images/GyIlxwsk/business-images/xT4oZeOQNedZVOuypAlv_moon.png">
                    <p class="time">{{ handleSet(cityData.sys.sunset) }} PM</p>
                </div>
            </div>
            <p class="asterisk">*Times reflect your timezone</p>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        cityName: {
            type: String,
            required: true
        },
        cityData: {
            type: Object,
            required: true
        },
    },
    data() {
        return {
             countries: {
                 US: 'https://s3.amazonaws.com/jebbit-assets/images/GyIlxwsk/business-images/JuUFwILzQRu17HpVNfqY_us.png',
                 CA: 'https://s3.amazonaws.com/jebbit-assets/images/GyIlxwsk/business-images/Y6eqALA6RUyxPwE5nKwM_canada.png',
                 QA: 'https://s3.amazonaws.com/jebbit-assets/images/GyIlxwsk/business-images/kWGQGoT9SbaiKEYSf9i3_qatar.png',
                 DE: 'https://s3.amazonaws.com/jebbit-assets/images/GyIlxwsk/business-images/xphgSoCBRgy9KV0KOZRp_germany.png'
             }
        }
    },
    computed: {
        weatherClass() {
            let cityForecast = this.cityData.weather[0].description;
         
            return {
                clearSky: cityForecast.match(/clear/i), 
                cloudy: cityForecast.match(/overcast/i) || cityForecast.match(/cloud/i), 
                misty: cityForecast.match(/mist/i), 
                drizzle: cityForecast.match(/drizzle/i),
                rain: cityForecast.match(/rain/i)
            }
        },
    },
    methods: {
        handleSet(time) {
            let unix_timestamp = time;
            var date = new Date(unix_timestamp * 1000);
            var hours = date.getHours();
            var minutes = "0" + date.getMinutes();
            var seconds = "0" + date.getSeconds();
            var formattedTime = hours + ':' + minutes.substr(-2) + ':' + seconds.substr(-2);
            return formattedTime;
        }
    }
}
</script>

<style>
    .city-card,
    .set-container,
    .set-sub-container {
        display: flex;
        align-items: center;
        justify-content: center
    }
    .card-container {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 200px;
        width: 100%;
        max-width: 500px;
        margin: 40px 0 0;
        border: 1px solid rgba(0,0,0,.5);
    }
    .row {
        height: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
    .row-1 {
        flex: 1;
    }
    .row-2 {
        flex: 2;
        display: flex;
        align-items: flex-start;
        justify-content: center;
    }
    .row-2 p {
        margin: 5px 0 5px 10px;
    }
    .row-2 .time {
        margin: 5px 0 5px 3px;
        font-size: 0.8em;
    }
    .city {
        background-color: #9966ff;
        color: #fff;
        width: 200px;
        flex: 1;
        margin: 0;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .country {
        height: 20px;
        width: auto;
        margin: 0 0 0 3px;
    }
    .image {
        flex: 4;
        background-size: cover;
        background-position: center;
        width: 100%;
    }
    .set-icon {
        height: 20px;
        width: auto;
        margin: 0 0 0 10px;
    }
    .clearSky {
        background-image: url(https://s3.amazonaws.com/jebbit-assets/images/GyIlxwsk/business-images/zkdQMSlETXOCzhEV6ugh_clear-sky.png);
    }
    .cloudy {
        background-image: url(https://s3.amazonaws.com/jebbit-assets/images/GyIlxwsk/business-images/UFjrWZXmReOVIXzhCm2Z_clouds.jpeg);
    }
    .misty {
        background-image: url(https://s3.amazonaws.com/jebbit-assets/images/GyIlxwsk/business-images/ffdcayLAQJKL927Nqqgx_misty.jpeg);
    }
    .brokenClouds {
        background-image: url(https://s3.amazonaws.com/jebbit-assets/images/GyIlxwsk/business-images/j9joPihTmCB3NmK6GLUt_broken-clouds.jpeg);
    }
    .drizzle {
        background-image: url(https://s3.amazonaws.com/jebbit-assets/images/GyIlxwsk/business-images/cfhsdweQE6DGguoPSfxK_drizzle-2.jpeg);
    }
    
    .rain {
        background-image: url(https://s3.amazonaws.com/jebbit-assets/images/GyIlxwsk/business-images/k7hxRVhTQxawXMsOCQ3h_light-rain.png);
        background-color: #0066ff;
        background-position: top;
    }
    .asterisk {
        font-size: 0.7em;
    }
</style>