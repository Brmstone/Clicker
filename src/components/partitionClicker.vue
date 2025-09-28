<template>
  <div class="cookie" @click="$emit('clicked')">
    <img src="../image/Ludo.PNG" alt="partition" />
  </div>
</template>

<script>
import partImg from "@/image/Ludo.PNG"
export default {
  name: "PartitionButton"
};
</script>

<style scoped>
.cookie img {
  width: 200px;
  height: 200px;
  cursor: pointer;
  transition: transform 0.1s;
}
.cookie img:active {
  transform: scale(0.95);
}
</style>


