<template>
  <div class="container">
    <h2>Receptek böngészése</h2>
    <ul class="list-group">
      <li v-for="recept in receptek" :key="recept.id" class="list-group-item d-flex justify-content-between">
        {{ recept.nev }}
        <button @click="fetchReceptDetails(recept.id)" class="btn btn-warning">Megtekintés</button>
      </li>
    </ul>
    <div v-if="kivalasztottRecept" class="card mt-4 p-3">
      <h3>{{ kivalasztottRecept.nev }}</h3>
      <p><strong>Elkészítési idő:</strong> {{ kivalasztottRecept.ido }} perc</p>
      <p><strong>Nehézség:</strong> {{ kivalasztottRecept.nehezseg }}</p>
      <p><strong>Leírás:</strong> {{ kivalasztottRecept.leiras }}</p>
      <h4>Hozzávalók:</h4>
      <ul>
        <li v-for="hozzavalo in kivalasztottRecept.hozzavalok" :key="hozzavalo.id">
          {{ hozzavalo.nev }} - {{ hozzavalo.mennyiseg }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      receptek: [],
      kivalasztottRecept: null
    };
  },
  methods: {
    async fetchReceptek() {
      const response = await fetch('http://localhost/api/recept');
      this.receptek = await response.json();
    },
    async fetchReceptDetails(id) {
      const response = await fetch(`http://localhost/api/recept?id=${id}`);
      this.kivalasztottRecept = await response.json();
    }
  },
  mounted() {
    this.fetchReceptek();
  }
};
</script>
