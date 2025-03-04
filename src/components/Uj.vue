<template>
  <div class="container">
    <h2>Új recept hozzáadása</h2>
    <form @submit.prevent="submitRecept">
      <label>Név:</label>
      <input v-model="ujRecept.nev" type="text" class="form-control" required />

      <label>Elkészítési idő (perc):</label>
      <input v-model="ujRecept.ido" type="number" class="form-control" required />

      <label>Nehézség:</label>
      <input v-model="ujRecept.nehezseg" type="text" class="form-control" required />

      <label>Elkészítés útmutató:</label>
      <textarea v-model="ujRecept.leiras" class="form-control" required></textarea>

      <h4>Hozzávalók:</h4>
      <button type="button" @click="addHozzavalo" class="btn btn-warning">+</button>
      <div v-for="(hoz, index) in ujRecept.hozzavalok" :key="index" class="input-group mt-2">
        <select v-model="hoz.id" class="form-select">
          <option v-for="hozzavalo in hozzavalokLista" :key="hozzavalo.id" :value="hozzavalo.id">
            {{ hozzavalo.nev }}
          </option>
        </select>
        <input v-model="hoz.mennyiseg" type="text" class="form-control" placeholder="Mennyiség" />
        <button type="button" @click="removeHozzavalo(index)" class="btn btn-danger">X</button>
      </div>

      <button type="submit" class="btn btn-warning mt-3">Recept mentése</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ujRecept: {
        nev: '',
        ido: '',
        nehezseg: '',
        leiras: '',
        hozzavalok: []
      },
      hozzavalokLista: []
    };
  },
  methods: {
    async fetchHozzavalok() {
      const response = await fetch('http://localhost/api/hozzavalo');
      this.hozzavalokLista = await response.json();
    },
    addHozzavalo() {
      this.ujRecept.hozzavalok.push({ id: '', mennyiseg: '' });
    },
    removeHozzavalo(index) {
      this.ujRecept.hozzavalok.splice(index, 1);
    },
    async submitRecept() {
      await fetch('http://localhost/api/recept', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(this.ujRecept)
      });
    }
  },
  mounted() {
    this.fetchHozzavalok();
  }
};
</script>
