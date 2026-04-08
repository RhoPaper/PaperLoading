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
</script>

<template>
    <!-- 容器：定位与 is-loading class 绑定 -->
    <div id="pixel-loading" :class="{ 'is-loading': isLoading }">
        <!-- 实际执行伸展动画的像素块 -->
        <div class="pixel"></div>
    </div>
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
}

/* 先横向扩展（左->右），再纵向扩展（下->上） */
#pixel-loading.is-loading .pixel {
    animation:
        expandWidth 0.5s ease forwards,
        expandHeight 0.5s ease 0.5s forwards;
}

/* 宽度：从初始像素扩展到容器宽度（右侧伸展） */
@keyframes expandWidth {
    from {
        width: var(--initial-size);
    }

    to {
        width: 100%;
    }
}

/* 高度：从初始像素扩展到容器高度（底部固定，向上生长） */
@keyframes expandHeight {
    from {
        height: var(--initial-size);
    }

    to {
        height: 100%;
    }
}
</style>