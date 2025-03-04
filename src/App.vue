<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-dark" style="background-color: rgb(88, 88, 88);">
      <div class="container">
        <a class="navbar-brand" href="#" @click.prevent="currentView = 'home'">
          <img src="/logo.png" alt="Logo" width="150" />
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav mx-auto">
            <li class="nav-item">
              <a href="#" class="nav-link" @click.prevent="currentView = 'uj'">Új recept hozzáadása</a>
            </li>
            <li class="nav-item">
              <a href="#" class="nav-link" @click.prevent="currentView = 'bongeszes'">Receptek böngészése</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <div class="container mt-4">
      <Uj v-if="currentView === 'uj'" />
      <Bongeszes v-if="currentView === 'bongeszes'" />
      <div v-if="currentView === 'home'">
        <h2>Üdvözöljük a receptkezelő alkalmazásban!</h2>
      </div>
    </div>
  </div>
</template>

<script>
import Uj from "./Components/Uj.vue";
import Bongeszes from "./Components/Bongeszes.vue";

export default {
  name: "App",
  components: { Uj, Bongeszes },
  data() {
    return {
      currentView: "home",
    };
  }
};
</script>
