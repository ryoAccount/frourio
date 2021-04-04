<template>
  <div>
    <svg
      class="logo"
      xmlns="http://www.w3.org/2000/svg"
      width="80"
      height="60"
      viewBox="0 0 24 24"
      fill="none"
      stroke="#000000"
      stroke-width="2"
      stroke-linecap="round"
      stroke-linejoin="round"
    >
      <path
        d="M20 14.66V20a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V6a2 2 0 0 1 2-2h5.34"
      ></path>
      <polygon points="18 2 22 6 12 16 8 16 8 12 18 2"></polygon>
    </svg>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend()
</script>

<style>
.logo {
  animation: 0.5s appear;
  margin: auto;
}

@keyframes appear {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
