<template>
    <div class="page">
        <div class="card-wrapper" ref="wrap">
            <AniCard v-model="isActive" />
        </div>
        <div class="lottie-wrapper">
            <AniGift v-model="isActive" />
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import AniCard from "./components/AniCard.vue";
import AniGift from "./components/AniGift.vue";

const isActive = ref(false);
const wrap = ref(null);

let x = 0;
let y = 0;
let mx = 0;
let my = 0;
let orientNumX = 0;
let orientNumY = 0;
let perNum = 20;

function loop() {
    mx += (x - mx) * 0.1;
    my += (y - my) * 0.1;
    wrap.value.style.transform =
        "translate(0, 0) rotateX(" +
        my / perNum +
        "deg) rotateY(" +
        -mx / perNum +
        "deg)";
    window.requestAnimationFrame(loop);
}

function mobileCheck() {
    const mobileKeyWords = new Array(
        "Android",
        "iPhone",
        "iPod",
        "BlackBerry",
        "Windows CE",
        "SAMSUNG",
        "LG",
        "MOT",
        "SonyEricsson",
    );
    for (let info in mobileKeyWords) {
        if (navigator.userAgent.match(mobileKeyWords[info]) != null)
            return true;
    }
    return false;
}

function init() {
    const isMobile = mobileCheck();
    if (isMobile) {
        perNum = 0.8;
        if (window.DeviceOrientationEvent) {
            window.addEventListener(
                "deviceorientation",
                function (event) {
                    x = event.gamma;
                    y = event.beta - 50;
                },
                false,
            );
        }
    } else {
        window.addEventListener("mousemove", function (e) {
            x = e.clientX - window.innerWidth / 2;
            y = e.clientY - window.innerHeight / 2;
        });
    }
    loop();
}

onMounted(init);
</script>

<style lang="sass" scoped>
.page
    height: 100vh
    overflow: hidden
.card-wrapper,
.lottie-wrapper
    display: flex
.card-wrapper
    transform-style: preserve-3d
    perspective: 1500px
.lottie-wrapper
    display: flex
    :deep(.lottie-el)
        margin: 15vh auto 0
</style>
