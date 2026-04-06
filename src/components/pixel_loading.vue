<template>
    <div class="loader-container">
        <div v-if="showLoader" class="pixel-bar" :key="animationKey"></div>
    </div>

    <button class="replay-btn" @click="replay">
        重播动画
    </button>
</template>

<script setup>
import { ref, nextTick } from 'vue'

const showLoader = ref(true)
const animationKey = ref(0)

const replay = () => {
    showLoader.value = false
    nextTick(() => {
        animationKey.value++
        showLoader.value = true
    })
}
</script>

<style scoped>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

:global(body) {
    width: 100vw;
    height: 100vh;
    background: #ffffff;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
}

:global(#app) {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}

.loader-container {
    position: relative;
    height: 2px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.pixel-bar {
    height: 2px;
    background: #000000;
    transform-origin: center;
    animation: elongate 2s cubic-bezier(0.4, 0.0, 0.2, 1) forwards;
}

@keyframes elongate {
    0% {
        width: 2px;
    }

    100% {
        width: 10px;
    }
}

/* 重播按钮 */
.replay-btn {
    position: absolute;
    bottom: 100px;
    padding: 10px 24px;
    border: 1px solid #000;
    background: transparent;
    cursor: pointer;
    font-size: 12px;
    letter-spacing: 1px;
    transition: all 0.3s ease;
}

.replay-btn:hover {
    background: #000;
    color: #fff;
}
</style>