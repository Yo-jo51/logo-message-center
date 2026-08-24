<template>
  <nav class="navbar">
    <h1><span class="LOGO">LOGO</span> Message Center</h1>
  </nav>
</template>

<script setup></script>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 15vh;
  background-color: #4f6e4f;
  color: var(--porcelain);
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 40px;
  box-sizing: border-box;
  z-index: 1000;
}

h1 {
  font-size: 2.3rem;
  font-weight: 400;
  margin: 0;
  color: #abc8b1;
  letter-spacing: -0.5px;
}

h1 .LOGO {
  color: var(--porcelain);
  font-weight: 600;
}
</style>
