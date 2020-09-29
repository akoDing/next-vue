<template>
  <div>
    <h1>Reactive方式</h1>
    <h1>{{ count }}</h1>
    <h1>{{ double }}</h1>
    <h1>鼠标X坐标：{{ x }}</h1>
    <h1>鼠标Y坐标：{{ y }}</h1>
    <button @click="increase">+1</button>
    <h1>axios：{{ resultTeacher }}</h1>
    <p v-for="(item, i) in resultTeacher" v-bind:key="i">
      --索引值--{{ i }} --每一项--{{ item.name }}
    </p>
    <!-- Suspense - 异步请求 -->
    <Suspense>
      <template #default>
        <async-show />
        <!-- <dog-show /> -->
      </template>
      <template #fallback>
        <h1>Loading !...</h1>
      </template>
    </Suspense>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed, reactive, toRefs } from "vue";
import useMouseTracker from "./useMouseTracker.vue";
import useURLLoader from "./useURLLoader.vue";
import AsyncShow from "./AsyncShow.vue";
// import DogShow from "./DogShow.vue";

// 接口类型定义 DataProps 形状
interface DataProps {
  count: number;
  double: number;
  increase: () => void;
}

interface Teacher {
  birthday: Date;
  department: number;
  education: number;
  email: string;
  gender: number;
  id: string;
  name: string;
  password: string;
  phone: string;
  pid: string;
  post: string;
  title: string;
}

export default defineComponent({
  name: "CountReactive",
  components: { AsyncShow },
  // setup 方法 vue 组件的初始化入口方法
  setup() {
    // useMouseTracker();
    const { x, y } = useMouseTracker();
    const { result: resultTeacher } = useURLLoader<Teacher[]>("teacher/list");
    console.log(resultTeacher);
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
      x,
      y,
      resultTeacher,
    };
  },
});
</script>