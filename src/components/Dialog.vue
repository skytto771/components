<template>
  <div class="dialog_overlay" v-if="visible">
    <div class="dialog">
      <div class="dialog_header" :style="{'justifyContent':standardPosition('title',titlePosition)}">
        <slot name="header">
          <span>{{ title }}</span>
          <div class="dialog_titleClose" @click="closeDialog">
            <svg
              width="16"
              height="16"
              viewBox="0 0 16 16"
              stroke="currentColor"
            >
              <line x1="12" y1="4" x2="4" y2="12"></line>
              <line x1="4" y1="4" x2="12" y2="12"></line>
            </svg>
          </div>
        </slot>
      </div>
      <div class="dialog_body">
        <div v-if="left" class="dialog_sidebar left">
          <slot name="left"></slot>
        </div>
        <div class="dialog_main">
          <slot name="main">
            这是对话框
          </slot>
        </div>
        <div v-if="right" class="dialog_sidebar right">
          <slot name="right"></slot>
        </div>
      </div>
      <div class="dialog_footer" :style="{'justifyContent':standardPosition('btn',footerBtns)}">
        <div class="dialog_footer_btn" v-if="footerBtns">
          <div class="dialog_footer_btns" v-for="item,index in footerBtns" :key="index">
            <!-- button对象可包含按钮的宽、高，主题颜色颜色、名称 -->
            <button :style="setBtnStyle(item)" @click="item.function">{{ item.name }}</button>
          </div>
        </div>
        <div class="dialog_footer_btn" v-else>
          <button @click="closeDialog">关闭</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>

const props = defineProps({
  visible: Boolean,
  title: {
    type: String,
    default: "对话框标题",
  },
  left: {
    type: Boolean,
    default: false,
  },
  // 是否显示左侧主体
  right: {
    type: Boolean,
    default: false,
  },
  // 是否显示右侧主体
  footerBtns: {
    type: Array,
  },
  // 按钮列表 属性:type,name,size,color,
  footerPosition:{
    type: String,
  },
  // 按钮位置 right left center
  titlePosition:{
    type:String,
  }
  // 标题位置 right left center
});
console.log(props.footerBtns);


const emit = defineEmits(["update:visible"]);

function closeDialog() {
  emit("update:visible", false);
}

function standardPosition(type,value){
  if(type === 'btn'){
    if(value && value !== 'center'){
      return `flex-${value}`
    }
    if(value && value === 'center'){
      return 'center'
    }
    return 'flex-end'
  }
  if(type === 'title'){
    
    if(value && value !== 'center'){
      return `flex-${value}`
    }
    if(value && value === 'center'){
      return 'center'
    }
    return 'center'
  }
}
function setBtnStyle(item) {
  function formatSize(size) {
  if (typeof size === 'number') {
    return `${size}px`;
  } else if (typeof size === 'string') {
    
    if (size.endsWith('px') || size.endsWith('em') || size.endsWith('%') || size.endsWith('rem')) {
      return size;
    }
    if (!isNaN(size)) {
      return `${Number(size)}px`;
    }
  }
  return size || '';
}
  // 默认样式，适用于未指定类型或类型不匹配的情况
  const defaultStyle = {
    width: formatSize(item.width),    // 提供默认宽度
    height: formatSize(item.height) || '40px',  // 提供默认高度
    background: '#ffffff',            // 默认背景色
    color: '#000000',                 // 默认文字颜色
    border: '1px solid #cccccc',      // 默认边框
    borderRadius: '4px'               // 默认圆角
  };

  // 根据不同的类型修改样式
  if (item.type) {
    switch(item.type) {
      case 'primary':
        return {
          ...defaultStyle,
          background: 'linear-gradient(90deg, #4f89ff 0%, #4458fe 100%)', // 蓝色背景
          color: '#ffffff',           // 白色文字
          border: 'none'              // 无边框
        };
      case 'warning':
        return {
          ...defaultStyle,
          backgroundColor: '#E6A23C', // 黄色背景
          color: '#ffffff',           // 白色文字
          border: 'none'              // 无边框
        };
      default:
        // 如果没有匹配的类型，返回默认样式
        return defaultStyle;
    }
  }

  return defaultStyle;
}

</script>

<style scoped lang="scss">
.dialog_overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.dialog {
  background: #fff;
  width: 600px;
  border-radius: 8px;
  overflow: hidden;
  .dialog_header {
    display: flex;
    align-items: center;
    position: relative;
    padding: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid #E7EAF2;
    .dialog_titleClose {
      position: absolute;
      right: 20px;
      display: flex;
      justify-content: center;
      align-items: center;
      width: 16px;
      height: 16px;
      &:hover {
        color: #4f89ff;
      }
      cursor: pointer;
    }
  }

  .dialog_body {
    display: flex;
  }

  .dialog_sidebar {
    flex: 1;
    padding: 10px;
  }

  .dialog_main {
    flex: 2;
    padding: 10px;
  }

  .dialog_footer {
    display: flex;
    padding: 10px 20px;
    justify-content: flex-end;
    .dialog_footer_btn {
      display: flex;
      gap: 8px;
      right: 20px;
      button {
        box-sizing: border-box;
        outline: none;
        box-shadow: none;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        font-size: 12px;
        color: #fff;
      }
    }
  }
}
</style>
