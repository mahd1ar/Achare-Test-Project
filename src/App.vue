<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div id="root">

    <header>
      <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <div class="container-fluid">
          <a class="navbar-brand" href="#">
            <img src="../src/assets/logo.svg" alt="Logo" width="30" height="30" class="d-inline-block align-top">
            ACHARE
          </a>
          <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
          </button>
          <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">

              <li class="nav-item">
                <RouterLink class="nav-link" to="/about"> مشاهده آدرس</RouterLink>
              </li>
              <li class="nav-item">
                <RouterLink class="nav-link" to="/register">ثبت آدرس</RouterLink>
              </li>
            </ul>
          </div>
        </div>
      </nav>
    </header>

    <RouterView />
  </div>
</template>

<style  lang="scss">

#root {
 min-height: 100vh;
 display: flex;
 flex-direction: column;
}

main {
  flex-grow: 1;
  height: 100%;
  flex-direction: column;
  display: flex;
}

footer {
  margin-top: auto;
}

</style>
