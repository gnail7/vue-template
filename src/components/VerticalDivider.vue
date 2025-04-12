<template>
  <div class="vertical-divider-container">
    <!-- 顶部右侧突出的横线 -->
    <div class="horizontal-line top"></div>
    
    <!-- 垂直线 -->
    <div class="vertical-line"></div>
    
    <!-- 中间内容区 -->
    <div class="content-area">
      <slot>
        ---
      </slot>
    </div>
    
    <!-- 垂直线 -->
    <div class="vertical-line"></div>
    
    <!-- 底部右侧突出的横线 -->
    <div class="horizontal-line bottom"></div>
  </div>
</template>

<script>
export default {
  props: {
    lineHeight: {
      type: String,
      default: 'calc(50% - 18px)' // 减去按钮区域高度
    },
    showDefaultButton: {
      type: Boolean,
      default: true
    },
    buttonText: {
      type: String,
      default: '按钮'
    },
    lineColor: {
      type: String,
      default: '#ccc'
    },
    lineWidth: {
      type: String,
      default: '1px'
    },
    horizontalLineLength: {
      type: String,
      default: '20px'
    }
  }
}
</script>

<style scoped>
.vertical-divider-container {
  display: flex;
  flex-direction: column;
  align-items: center; /* 改为center让内容居中 */
  height: 100%;
  position: relative;
}

.vertical-line {
  width: v-bind(lineWidth);
  background: v-bind(lineColor);
  min-height: 20px;
  position: relative; /* 添加相对定位 */
}

.horizontal-line {
  width: 12px;
  height: v-bind(lineWidth);
  background: v-bind(lineColor);
  position: relative;
  margin-left: 12px;

}

.content-area {
  padding: 8px 0;
  display: flex;
  justify-content: center;
  width: 100%;
}

.divider-button {
  padding: 4px 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background: #f8f8f8;
  cursor: pointer;
  font-size: 12px;
}
</style>