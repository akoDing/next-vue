<template>
  <div>
    <h1>Reactive方式</h1>
    <h1>{{ count }}</h1>
    <h1>{{ double }}</h1>
    <button @click="increase">+1</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed, reactive, toRefs } from "vue";

// 接口类型定义
interface DataProps {
  count: number;
  double: number;
  increase: () => void;
}

export default defineComponent({
  name: "CountReactive",
  components: {},
  // setup 方法 vue 组件的初始化入口方法
  setup() {
    // reactive 函数方式 data. 取值
    const data: DataProps = reactive({
      count: 0,
      // 自定义 +1 方法
      increase: () => {
        data.count++;
      },
      // computed 计算属性
      double: computed(() => data.count * 2),
    });
    // toRefs 包含 reactive data
    // toRefs 保证 reactive 对象属性保持响应性
    const refData = toRefs(data);
    return {
      ...refData,
    };
  },
});
</script>