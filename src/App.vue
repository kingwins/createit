<template>
  <div id="app">
    <div>
      <b-navbar type="dark" variant="primary" class="py-3 goldnavbar">
        <div class="container">
          <b-navbar-nav>
            <b-nav-item href="/">Home</b-nav-item>
            <b-nav-item href="/about">About</b-nav-item>
          </b-navbar-nav>
        </div>
      </b-navbar>
    </div>
    <div class="main">
      <transition name="route">
        <router-view />
      </transition>
    </div>
  </div>
</template>

<style lang="scss" src="@/styles/main.scss"></style>
