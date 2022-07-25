<template>
    <div
        class="card"
        :class="{ 'is-active': isActive }"
        @click="isActive = !isActive"
    >
        <div class="card-page card-page-front">
            <div class="card-page card-page-outside"></div>
            <div class="card-page card-page-inside">
                <div class="card-page-inside__layer-1"></div>
            </div>
        </div>
        <div class="card-page card-page-bottom">
            <div class="card-content">
                <div class="card-to">致 Ray 和 Jeffery：</div>
                <div class="card-text">
                    <p>生日快樂！<br />明年繼續一起感謝生命的成長與餽贈吧。</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";
let props = defineProps(["modelValue"]);
let emits = defineEmits(["update:modelValue"]);

const isActive = computed({
    get() {
        return props.modelValue;
    },
    set(nVal) {
        emits("update:modelValue", nVal);
    },
});
</script>

<style lang="sass" scoped>
.card,
.card-page
    width: 300px
    height: 400px
    @media (max-width: 576px)
        width: calc(50vw - 10px)
        height: calc((50vw - 10px) * 1.333)
.card
    position: absolute
    left: calc(50% - 150px)
    top: 0
    transform-style: preserve-3d
    perspective: 1500px
    transition-property: transform, opacity
    transition-duration: .5s
    transition-delay: .5s
    opacity: 0
    transform: rotate(-30deg) translate(0, 25vh) scale(.5)
    &.is-active
        opacity: 1
        transform: rotate(0deg) translate(150px, 15vh) scale(1)
        transition-duration: 1s, .3s
        transition-delay: 1.5s, 1s
        .card-page-front
            transform: rotateY(-180deg)
            transition-delay: 1.5s
.card:before
    content: ""
    display: block
    position: absolute
    left: 0
    top: 0
    width: 100%
    height: 100%
    box-shadow: 0 0 300px #ccc
    background: white
    transition: all 0.5s ease-in-out

.card-page
    transition: transform 1s ease-in-out
    cursor: pointer
    position: absolute
    outline: 1px solid transparent

.card-page-front
    transform-origin: 0 50% 0
    transform-style: preserve-3d
    transform: rotateY(0deg)
    z-index: 2

.card-page-outside,
.card-page-inside
    position: absolute
    -webkit-backface-visibility: hidden

.card-page-outside
    border: 10px solid #fbfbfb
    background: #1165ae url("/card-image.jpg") no-repeat center
    background-size: cover
    width: 100%
    height: 100%

.card-page-inside,
.card-page-bottom
    background-color: #f9f9f9
    background-size: contain
    background-position: 0px 80px
    border: 20px solid #f9f9f9
.card-page-inside
    transform: rotateY(-180deg)
    border-right: none
    background-position: 0px 80px
    background-image: url("/card-pattern.jpg")
    transform-style: preserve-3d
    perspective: 200px
    &__layer-1
        width: 80%
        height: 100%
        margin: auto
        background-size: 100% auto
        background-repeat: no-repeat
        background-position: center
        background-image: url("/card-front-layer-1.png")
        transform: translateZ(60px)
.card-page-bottom
    z-index: 1
    border-left: none
    background-position: -22px 80px
    .card-content
        padding-left: 20px
        display: flex
        flex-direction: column
        height: 100%
        font-size: 18px
        @media (max-width: 576px)
            font-size: 14px
        .card-to
            margin-bottom: 48px
            @media (max-width: 576px)
                margin-bottom: 24px
        .card-text
            text-align: center
            flex: 1 0 auto
</style>
