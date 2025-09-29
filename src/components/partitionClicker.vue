<template>
  <div class="cookie" @click="$emit('clicked')">
    <img src="../image/Partition.png" alt="partition" />
  </div>
</template>

<script>
export default {
  name: "PartitionButton"
};
</script>

<style scoped>
.cookie img {
  width: 352px;
  height: 427px;
  cursor: pointer;
  transition: transform 0.1s;
}
.cookie img:active {
  transform: scale(0.95);
}
</style>


