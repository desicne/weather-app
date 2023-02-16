<template>
    <div class="main-data">
        <h1><span class="helper"><u>weather</u></span><br> {{ city }}</h1>
        <animated-number
            class="temperature"
            :value="temp"
            :round="1"
            :duration="2000"
        />
        <span class="degrees">°</span>
        <p>{{ overall }}</p>
    </div>
</template>
<script>
import AnimatedNumber from 'animated-number-vue';
export default {
    props: ['city', 'temp', 'overall'],
    components: {
        AnimatedNumber
    },
}
</script>
<style lang="scss" scoped>
h1 {
    font-size: 24px;
}

.main-data {
    text-align: center;
}
.helper {
    font-size: 12px;
    font-weight: 400;
    text-transform: uppercase;
}
.temperature {
    padding: 16px 0 0;
    font-size: 10rem;
    font-weight: 800;
}

.degrees {
    position: relative;
    top: -5rem;
    font-size: 40px;
}
</style>