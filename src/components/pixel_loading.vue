<script setup>
import { ref, onMounted, onUnmounted } from 'vue'; // 引入响应式与生命周期

const isRunning = ref(false); // 动画当前是否展开
let animationTimer = null; // 存储定时器 id
const PAUSE_DURATION = 500; // 暂停时长（毫秒）

function expandAnimation() {
    isRunning.value = true; // 切换为展开状态
    animationTimer = setTimeout(() => { // 等待展开完成
        pauseThenCollapse(); // 进入暂停再收缩
    }, 700); // 展开动画时长
}

function pauseThenCollapse() {
    animationTimer = setTimeout(() => { // 暂停一段时间后
        collapseAnimation(); // 启动收缩动画
    }, PAUSE_DURATION); // 使用统一暂停时长
}

function collapseAnimation() {
    animationTimer = setTimeout(() => { // 执行收缩动画
        isRunning.value = false; // 标记为收缩完成
        animationTimer = setTimeout(() => { // 收缩后再次暂停
            expandAnimation(); // 循环回到展开
        }, PAUSE_DURATION); // 收缩后暂停时长
    }, 300); // 收缩动画时长
}

function start() {
    if (animationTimer) clearTimeout(animationTimer); // 清除已有定时器
    expandAnimation(); // 启动动画循环
}

function stop() {
    isRunning.value = false; // 立即设置为收缩状态
    if (animationTimer) { // 若有定时器则清除
        clearTimeout(animationTimer); // 清除定时器
        animationTimer = null; // 重置定时器引用
    }
}

onMounted(() => { // 组件挂载时
    start(); // 自动开始动画
});

onUnmounted(() => { // 组件卸载时
    stop(); // 停止并清理
});
</script>

<template>
    <!-- 容器：类由 isRunning 控制 -->
    <div id="pixel-loading" :class="{ expand: isRunning, collapse: !isRunning }">
        <!-- 单个像素：执行展开/收缩动画 -->
        <div class="pixel"></div>
    </div>
    <!-- 按钮：手动开始动画 -->
    <button @click="start">Start</button>
    <!-- 按钮：手动停止动画 -->
    <button @click="stop">Stop</button>
</template>

<style scoped>
#pixel-loading {
    /* 最终宽度变量 */
    --final-width: 50px;
    /* 最终高度变量 */
    --final-height: 50px;
    /* 像素初始尺寸变量 */
    --initial-size: 5px;

    /* 绝对定位容器 */
    position: absolute;
    /* 水平居中基点 */
    left: 50%;
    /* 向左平移 50% 以居中 */
    transform: translateX(-50%);
    /* 变换基点设置为中心 */
    transform-origin: center center;
    /* 垂直居中（减去一半高度） */
    bottom: calc(50% - var(--final-height) / 2);
    /* 容器尺寸：使用变量 */
    width: var(--final-width);
    height: var(--final-height);
    /* 背景透明 */
    background: transparent;
    /* 允许动画溢出可见 */
    overflow: visible;
}

.pixel {
    /* 像素为绝对定位相对容器 */
    position: absolute;
    /* 靠左对齐 */
    left: 0;
    /* 靠下对齐 */
    bottom: 0;
    /* 初始宽度 */
    width: var(--initial-size);
    /* 初始高度 */
    height: var(--initial-size);
    /* 像素颜色 */
    background-color: #000;
    /* 变换基点为左下角 */
    transform-origin: left bottom;
    /* 提示浏览器优化宽高动画 */
    will-change: width, height;
}

#pixel-loading.expand .pixel {
    /* 展开时使用 expand 动画 */
    animation: expand 0.7s cubic-bezier(0.65, 0, 0.35, 1) forwards;
}

#pixel-loading.collapse .pixel {
    /* 收缩时使用 collapse 动画 */
    animation: collapse 0.3s cubic-bezier(0.37, 0.82, 0.21, 0.97) forwards;
}

@keyframes expand {
    0% {
        /* 初始状态：小像素 */
        width: var(--initial-size);
        height: var(--initial-size);
    }

    50% {
        /* 中间状态：横向拉伸 */
        width: 100%;
        height: var(--initial-size);
    }

    100% {
        /* 结束状态：填满容器 */
        width: 100%;
        height: 100%;
    }
}

@keyframes collapse {
    0% {
        /* 初始：填满容器 */
        width: 100%;
        height: 100%;
    }

    50% {
        /* 中间：再次横向拉伸但高度回到初始 */
        width: 100%;
        height: var(--initial-size);
    }

    100% {
        /* 结束：回到像素大小 */
        width: var(--initial-size);
        height: var(--initial-size);
    }
}
</style>