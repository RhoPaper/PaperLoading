<script setup>
import { ref, onMounted } from 'vue';

// 控制加载状态，绑定到模板 class
const isLoading = ref(false);

// 组件挂载后触发动画（示例）
// 1100ms 用于等待两段 0.5s 动画完成后执行回调
onMounted(() => {
    isLoading.value = true;
    setTimeout(() => {
        console.log('Loading completed');
        // 可在此处重置 isLoading 或执行其它后续逻辑
    }, 1100);
});

function testLoadingAnimation() {
    isLoading.value = true;
    setTimeout(() => {
        console.log('Loading completed');
    }, 1100);
}
</script>

<template>
    <!-- 容器：定位与 is-loading class 绑定 -->
    <div id="pixel-loading" :class="{ 'is-loading': isLoading }">
        <!-- 实际执行伸展动画的像素块 -->
        <div class="pixel"></div>
    </div>
    <button @click="testLoadingAnimation()">Test Loading Animation</button>
</template>

<style scoped>
#pixel-loading {
    /* 最终尺寸与初始像素大小 */
    --final-width: 50px;
    --final-height: 50px;
    --initial-size: 5px;

    position: absolute;
    /* 水平居中（基于元素中心） */
    left: 50%;
    transform: translateX(-50%);
    transform-origin: center center;

    /* 垂直中心按最终高度计算 */
    bottom: calc(50% - var(--final-height) / 2);

    /* 将容器固定为最终尺寸，内部像素块在此容器内伸展 */
    width: var(--final-width);
    height: var(--final-height);
    background: transparent;
    overflow: visible;
}

/* 像素块初始状态：小方块，靠左并与底部对齐 */
.pixel {
    position: absolute;
    left: 0;
    bottom: 0;
    /* 固定底部以实现自下而上生长 */
    width: var(--initial-size);
    height: var(--initial-size);
    background-color: #000;
    transform-origin: left bottom;
    /* 宽度以左为锚，垂直以底为锚 */

    /* 性能优化提示，提升动画丝滑度 */
    will-change: width, height;
}

/* 使用丝滑的自定义缓动曲线并保持无限循环 */
#pixel-loading.is-loading .pixel {
    /* 使用更丝滑的非线性缓动：
       cubic-bezier(0.22, 1, 0.36, 1) 常用于产生平滑且略有弹性的过渡 */
    animation: pixelLoop 2.5s cubic-bezier(0.22, 1, 0.36, 1) infinite;
    /* 兼容性备选（浏览器可能忽略）： */
    animation-timing-function: cubic-bezier(0.22, 1, 0.36, 1);
}

/* 使用单一 keyframes 完成前进 -> 等待1s -> 反向 -> 收回后等待0.5s，并无限循环 */
@keyframes pixelLoop {
    from {
        width: var(--initial-size);
        height: var(--initial-size);
    }

    14% {
        width: 100%;
        height: var(--initial-size);
    }

    28% {
        width: 100%;
        height: 100%;
    }

    68% {
        width: 100%;
        height: 100%;
    }

    74% {
        width: 100%;
        height: var(--initial-size);
    }

    80% {
        width: var(--initial-size);
        height: var(--initial-size);
    }

    to {
        /* 保持小方块直到下一周期开始（80% -> 100% 为 0.5s 等待） */
        width: var(--initial-size);
        height: var(--initial-size);
    }
}
</style>