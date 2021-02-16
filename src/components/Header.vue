<template>
  <div :class="`flex justify-between bg-green-900 p-4 items-center ${height}`">
    <div class="title text-3xl text-white">
      <strong>TODO</strong><span class="font-thin">LIST</span>
    </div>
    <div class="logo">
    </div>
  </div>
</template>

<script>
export default {
props: {
  height: {
    type: String,
    default: 'h-16'
  }
}
}
</script>

<style>

</style>