<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>

    <div class="wrapper">

      <nav class="nav" id="nav">
        <div class="nav-inner">
          <div class="nav-header">
          Syracuse Athletics Notable Alumni
          </div>
          <div class="nav-links">
            <a class="nav-logo" href="#top">
              <img src="/main_nav_logo.svg" alt="syracuse logo">
            </a>
            <RouterLink to="/" class="nav-link">Athletes</RouterLink>
            <RouterLink to="/about" class="nav-link">About</RouterLink>
          </div>
        </div>
      </nav>
    </div>
  </header>
  <div id="top"></div>
  <RouterView />
</template>

<style scoped>

</style>
