<script setup>
import { ref, onMounted } from 'vue';

const isLoading = ref(false);

onMounted(() => {
    setTimeout(() => {
        isLoading.value = true;
    }, 100);

    setTimeout(() => {
        console.log('Loading completed');
    }, 1100);
});
</script>

<template>
    <div id="pixel-loading" :class="{ 'is-loading': isLoading }"></div>
</template>

<style scoped>
#pixel-loading {
    --final-width: 100px;
    --final-height: 100px;
    --initial-size: 10px;

    position: absolute;
    /* 水平：左边缘对齐中心，向右展开 */
    left: 50%;
    transform: translate(0, 0);
    /* 垂直：底部定位在视口中心 + 最终高度的一半，这样展开后中心点才居中 */
    bottom: calc(50% - var(--final-height) / 2);

    width: var(--initial-size);
    height: var(--initial-size);
    background-color: #000;
}

#pixel-loading.is-loading {
    animation:
        expandWidth 0.5s ease forwards,
        expandHeight 0.5s ease 0.5s forwards;
}

@keyframes expandWidth {
    from {
        width: var(--initial-size);
    }

    to {
        width: var(--final-width);
    }
}

@keyframes expandHeight {
    from {
        height: var(--initial-size);
    }

    to {
        height: var(--final-height);
    }
}
</style>