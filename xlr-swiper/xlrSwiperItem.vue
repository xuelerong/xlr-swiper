<template>
    <div class="switemItems">
        <slot></slot>
    </div>
</template>

<script>
  export default {
    name: "xlrSwiperItem"
  }
</script>

<style scoped>
    .switemItems {
        width: 100%;
        flex-shrink: 0
    }

    .switemItems img {
        width: 100%;
    }


</style>