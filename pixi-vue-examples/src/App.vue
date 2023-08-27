<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <div class="wrapper">
      <nav>
        <RouterLink to="/">Home</RouterLink> |
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped lang="less">
.wrapper {
  width: 100%;
  margin: 10px auto;
  display: flex;
  justify-content: center;
  a {
    margin: 0 5px;
  }
  :deep(.router-link-active) {
    color: green;
  }
}
</style>
