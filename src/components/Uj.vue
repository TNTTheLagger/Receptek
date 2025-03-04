<template>
  <div class="container text-light p-4 rounded">
    <h2 class="text-warning text-center">Új recept hozzáadása</h2>
    <form @submit.prevent="submitRecept">
      <div class="mb-3">
        <label class="text-warning form-label">Név:</label>
        <input v-model="ujRecept.nev" type="text" class="form-control" required />
      </div>

      <div class="mb-3">
        <label class="text-warning form-label">Elkészítési idő (perc):</label>
        <input v-model="ujRecept.ido" type="number" class="form-control" required />
      </div>

      <div class="mb-3">
        <label class="text-warning form-label">Nehézség:</label>
        <input v-model="ujRecept.nehezseg" type="text" class="form-control" required />
      </div>

      <div class="mb-3">
        <label class="text-warning form-label">Elkészítés útmutató:</label>
        <textarea v-model="ujRecept.leiras" class="form-control" required></textarea>
      </div>

      <h4 class="text-warning">Hozzávalók:</h4>
      <button type="button" @click="addHozzavalo" class="btn btn-warning mb-2">+</button>

      <div v-for="(hoz, index) in ujRecept.hozzavalok" :key="index" class="input-group mb-2">
        <select v-model="hoz.id" class="form-select" @change="updateMertekegyseg(index)">
          <option v-for="hozzavalo in hozzavalokLista" :key="hozzavalo.id" :value="hozzavalo.id">
            {{ hozzavalo.nev }}
          </option>
        </select>
        <input v-model="hoz.mennyiseg" type="text" class="form-control" />
        <span class="input-group-text">{{ getMertekegyseg(hoz.id) }}</span>
        <button type="button" @click="removeHozzavalo(index)" class="btn btn-danger">X</button>
      </div>

      <button type="submit" class="btn btn-warning w-100 mt-3">Recept mentése</button>
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
    getMertekegyseg(id) {
      const hozzavalo = this.hozzavalokLista.find(item => item.id === id);
      return hozzavalo ? hozzavalo.mertekegyseg : '';
    },
    async submitRecept() {
      await fetch('http://localhost/api/recept', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(this.ujRecept)
    })}
  },
  mounted() {
    this.fetchHozzavalok();
  }
};
</script>
