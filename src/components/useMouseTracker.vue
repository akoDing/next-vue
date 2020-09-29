<script lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
// 将组件内逻辑抽象成可复用的函数
function useMouseTracker() {
  // const positions = reactive<MousePostion>({
  //   x: 0,
  //   y: 0
  // })
  const x = ref(0);
  const y = ref(0);
  const updatePosition = (event: MouseEvent) => {
    x.value = event.clientX;
    y.value = event.clientY;
  };
  onMounted(() => {
    document.addEventListener("click", updatePosition);
  });
  onUnmounted(() => {
    document.removeEventListener("click", updatePosition);
  });
  return { x, y };
}

export default useMouseTracker;
</script>