<template>
  <div>
    <button @click="openModal">Open Modal</button><br />
    <Modal :isOpen="modalIsOpen" @close-modal="onModalClose">
      My Modal !!!!</Modal
    >
    <h1>Ref方式</h1>
    <h1>{{ count }}</h1>
    <h1>{{ double }}</h1>
    <button @click="increase">+1</button>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent, computed, Ref, onMounted, onUpdated } from "vue";
import Modal from "./Modal.vue";

export default defineComponent({
  name: "CountsRef",
  components: { Modal },
  // setup 方法 vue 组件的初始化入口方法
  setup() {
    const modalIsOpen = ref(false);
    const openModal = () => {
      modalIsOpen.value = true;
    };
    const onModalClose = (ev: string) => {
      console.log(ev);
      modalIsOpen.value = false;
    };
    // ref 是一个函数，它接受一个参数，返回的就是一个神奇的响应式对象。我们初始化的这个 0 作为参数包裹到这个对象中去，在未来可以检测到改变并作出对应的相应。
    const count: Ref<number> = ref(0);
    const double = computed(() => {
      return count.value * 2;
    });
    // 自定义 +1 方法
    const increase = () => {
      count.value++;
    };
    // 初始化取值方法
    onMounted(() => {
      console.log(count.value);
    });
    onUpdated(() => {
      console.log(count.value);
    });
    // 需要把值和方法返回出去
    return {
      count,
      increase,
      double,
      openModal,
      onModalClose,
      modalIsOpen,
    };
  },
});
</script>