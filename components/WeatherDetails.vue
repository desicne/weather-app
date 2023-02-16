<template>
    <div class="weather-details-holder">
        <div>
            <p>💦 Humidity</p>
            <p class="bold">{{ weather.main.humidity }}%</p>
        </div>
        <div>
            <p>💨 Wind</p>
            <p class="bold">{{ weather.wind.speed }}m/s</p>
        </div>
        <div>
            <p>🌞 Sunrise</p>
            <p class="bold">{{ formatTime(weather.sys.sunrise) }}</p>
        </div>
        <div>
            <p>🌇 Sunset</p>
            <p class="bold">{{formatTime(weather.sys.sunset) }}</p>
        </div>
    </div>
</template>
<script>
export default {
    props: ['weather'],
    methods: {
        formatTime(unix) {
            var date = new Date(unix * 1000);
            var hours = date.getHours();
            var minutes = '0' + date.getMinutes();
            return hours + ':' + minutes.substr(-2)
        }
    }
}
</script>
<style lang="scss" scoped>
div > p {
    display: inline-block;
    padding: 8px;
    min-width: 100px;
    text-transform: uppercase;
    font-size: 12px;
}

.weather-details-holder {
    position: absolute;
    left: 32px;
    bottom: 32px;
}
</style>