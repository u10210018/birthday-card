<template>
    <div
        class="lottie-el"
        :class="{ 'is-active': isActive }"
        ref="lottieEl"
        @click="boxClickHandler"
    ></div>
</template>

<script setup>
import { ref, onMounted, computed, watch } from "vue";
import lottie from "lottie-web";
import animationData from "../assets/json/birthday.json";

let props = defineProps(["modelValue"]);
let emits = defineEmits(["update:modelValue"]);

const lottieEl = ref(null);
const lottieAni = ref(null);

async function lottieInit() {
    lottieAni.value = lottie.loadAnimation({
        container: lottieEl.value,
        renderer: "svg",
        loop: false,
        autoplay: false,
        animationData,
    });
    lottieAni.value.goToAndStop(21, true);
}

function boxClickHandler() {
    isActive.value = !isActive.value;
}

const isActive = computed({
    get() {
        return props.modelValue;
    },
    set(nVal) {
        emits("update:modelValue", nVal);
    },
});

watch(
    () => isActive.value,
    (nVal) => {
        if (!nVal) lottieAni.value.playSegments([[67, 21]], true);
        else lottieAni.value.playSegments([[21, 67]], true);
    },
);

onMounted(lottieInit);
</script>

<style lang="sass" scoped>
.lottie-el
    width: 50vmin
    height: 50vmin
    cursor: pointer
    transform: translateY(0) scale(1.5)
    transition: transform 1.5s ease-in-out .3s
    &.is-active
        transform: translateY(35vh) scale(.85)
</style>
