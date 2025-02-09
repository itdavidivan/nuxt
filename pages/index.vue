<template>
  <div class="container">
    <nuxt-link to="/employees" class="link">Employees</nuxt-link>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "IndexPage",
});
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f4f8;
  padding: 20px;
  font-family: "Arial", sans-serif;
}

.link {
  text-decoration: none;
  font-size: 1.5rem;
  font-weight: bold;
  color: #3498db;
  background-color: #ffffff;
  padding: 12px 24px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.link:hover {
  background-color: #3498db;
  color: white;
  transform: translateY(-4px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
}

@media (max-width: 768px) {
  .link {
    font-size: 1.2rem;
    padding: 10px 20px;
  }
}
</style>
