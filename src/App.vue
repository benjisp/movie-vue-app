<template>
  <div id="app">
    <div class="tm-blue-bg tm-left-column">
      <div class="tm-logo-div text-xs-center">
        <img src="img/tm-neaty-logo.png" alt="Logo" />
        <h1 class="tm-site-name">Neaty</h1>
      </div>
      <nav class="tm-main-nav">
        <ul class="tm-main-nav-ul">
          <li class="tm-nav-item">
            <router-link to="/" class="tm-nav-item-link">All Actors</router-link>
          </li>
          <li class="tm-nav-item">
            <router-link to="/movies" class="tm-nav-item-link">All Movies</router-link>
          </li>
          <li class="tm-nav-item">
            <router-link to="movies/new" class="tm-nav-item-link">New Movie</router-link>
          </li>
        </ul>
      </nav>
    </div>
    <router-view />
  </div>
</template>

<style></style>
