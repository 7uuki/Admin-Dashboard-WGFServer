<script setup lang="ts">
</script>

<template>
  <h1>WGF-Admin-Dashboard</h1>
  <div>
    <a href="https://nginx.monou.de" target="_blank">
      <img src="./assets/NginxProxyManager.png" class="logo" alt="NginxProxyManager logo" />
    </a>
    <a href="https://ci.umgefall.de" target="_blank">
      <img src="./assets/Jenkins.svg" class="logo jenkins" alt="Jenkins logo" />
    </a>
    <a href="https://docker.umgefall.de" target="_blank">
      <img src="./assets/Portainer.svg" class="logo portainer" alt="Portainer logo" />
    </a>
  </div>

  <p>
    This is a simple Dashboard to navigate to the different services of the WGF Server.
  </p>
  <p>
    See the Code of this Project on
    <a
        href="https://github.com/7uuki/Admin-Dashboard-WGFServer"
        target="_blank"
    >Github</a
    >
  </p>
  <p class="read-the-docs">Click on the Logos to redirect</p>

</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #983790);
}
.logo.jenkins:hover {
  filter: drop-shadow(0 0 2em #d13833);
}
.logo.portainer:hover {
  filter: drop-shadow(0 0 2em #3bbbeb);
}
.read-the-docs {
  color: #888;
}
</style>
