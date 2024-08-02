<template>
  <!-- 展示外部图标 -->
  <div
    v-if="isExternal"
    :style="styleExternalIcon"
    class="svg-external-icon svg-icon"
    :class="className"
  ></div>
  <!-- 展示内部图标 -->
  <svg v-else class="svg-icon" :class="className" aria-hidden="true">
    <use :xlink:href="iconName" />
  </svg>
</template>
<script setup>
import { defineProps, computed } from 'vue'

import { isExternal as external } from '@/utils/validate'

const props = defineProps({
  // icon图标
  icon: {
    type: String,
    required: true,
  },
  // 图标类名
  className: {
    type: String,
    default: '',
  },
})
const isExternal = computed(() => external(props.icon))
// 外部图标
const styleExternalIcon = computed(() => ({
  mask: `url(${props.icon}) no-repeat 50% 50%`,
  '-webkit-mask': `url(${props.icon} no-repeat) 50% 50%`,
}))
// 内部图标
const iconName = computed(() => `#icon-${props.icon}`)
</script>
<style lang="scss" scoped>
.svg-icon {
  width: 1em;
  height: 1em;
  overflow: hidden;
  vertical-align: -0.15em;
  fill: currentcolor;
}

.svg-externa-icon {
  display: inline-box;
  background-color: currentcolor;
  mask-size: cover !important;
}
</style>
