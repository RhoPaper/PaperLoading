<script setup>
import { onMounted } from 'vue';

const afterLoadingWidth = '100px';
var isLoading = false;

const LoadingAnimation = (element, targetWidth) => {
    if (element && isLoading) {
        element.style.width = targetWidth;
    }
};

const onLoading = () => {
    let loadingDOM = window.document.getElementById("pixel-loading");
    setTimeout(() => {
        isLoading = true;
        LoadingAnimation(loadingDOM, afterLoadingWidth);
    }, 100);
    setTimeout(() => {
        console.log('Loading completed');
        isLoading = false;
    }, 1000);
};

onMounted(() => {
    onLoading();
});
</script>

<template>
    <div id="pixel-loading"></div>
</template>

<style scoped>
#pixel-loading {
    position: absolute;
    top: 50%;
    left: 50%;
    /* translateX(0) 让左边缘对齐中心点，宽度向右增长 */
    transform: translate(0, -50%);
    width: 10px;
    height: 10px;
    background-color: #000;
    transition: width .5s ease;
}
</style>
