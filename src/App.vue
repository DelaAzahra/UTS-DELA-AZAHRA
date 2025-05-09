<script setup>
import { ref, computed, onMounted } from 'vue';

const daftarTugas = ref([
  { nama: "Persiapkan bahan kue", selesai: false },
  { nama: "Buat adonan kue ulang tahun", selesai: true },
  { nama: "Cek pesanan online dari Shopee & WA", selesai: false },
  { nama: "Dekorasi kue dan kemas", selesai: true }
]);

const tugasBaru = ref("");
const inputTugas = ref(null);

const sedangEdit = ref(false);
const indeksEdit = ref(null);
const tampilkanSelesai = ref(true);

const tambahTugas = () => {
  const bersih = tugasBaru.value.trim();
  if (bersih) {
    daftarTugas.value.push({ nama: bersih, selesai: false });
    tugasBaru.value = "";
    inputTugas.value?.focus();
  }
};

const hapusTugas = (indeks) => {
  daftarTugas.value.splice(indeks, 1);
  batalEdit();
};

const editTugas = (indeks) => {
  tugasBaru.value = daftarTugas.value[indeks].nama;
  sedangEdit.value = true;
  indeksEdit.value = indeks;
  inputTugas.value?.focus();
};

const perbaruiTugas = () => {
  const bersih = tugasBaru.value.trim();
  if (indeksEdit.value !== null && bersih) {
    daftarTugas.value[indeksEdit.value].nama = bersih;
    batalEdit();
  }
};

const batalEdit = () => {
  tugasBaru.value = "";
  sedangEdit.value = false;
  indeksEdit.value = null;
};

const tugasTersaring = computed(() =>
  tampilkanSelesai.value
    ? daftarTugas.value
    : daftarTugas.value.filter(tugas => !tugas.selesai)
);

onMounted(() => {
  inputTugas.value?.focus();
});
</script>

  <template>
  <div id="app">
    <div class="container">
      <div class="card">
        <h2>🧁 Agenda Harian Toko Kue Dela</h2>

        <label class="toggle-show">
          <input type="checkbox" v-model="tampilkanSelesai" />
          Tampilkan yang sudah selesai
        </label>

        <ul class="task-list">
          <li v-for="(tugas, indeks) in tugasTersaring" :key="indeks">
            <div class="task-content">
              <input type="checkbox" v-model="tugas.selesai" />
              <span :class="{ done: tugas.selesai }">{{ indeks + 1 }}. {{ tugas.nama }}</span>
            </div>
            <div class="action-buttons">
              <button class="edit-btn" @click="editTugas(indeks)">✏</button>
              <button class="delete-btn" @click="hapusTugas(indeks)">🗑</button>
            </div>
          </li>
        </ul>

        <div class="input-group">
          <input
            ref="inputTugas"
            v-model="tugasBaru"
            type="text"
            placeholder="Tambahkan kegiatan toko kue"
          />
          <button v-if="sedangEdit" @click="perbaruiTugas">Perbarui</button>
          <button v-else @click="tambahTugas">Tambah</button>
          <button v-if="sedangEdit" class="cancel-btn" @click="batalEdit">Batal</button>
        </div>
      </div>
    </div>
  </div>
</template>


<style scoped>
html,
body,
#app {
  margin: 0;
  padding: 0;
  width: 100%;
  min-height: 100vh;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: #fff0f6; /* pink muda */
  color: #4b2c4b; /* ungu tua */
  display: flex;
  justify-content: center;
  align-items: flex-start;
}

.container {
  flex: 1;
  display: flex;
  justify-content: center;
  padding: 40px 20px;
  box-sizing: border-box;
}

.card {
  width: 100%;
  max-width: 520px;
  background: #ffffff;
  border-radius: 16px;
  padding: 30px 25px;
  box-shadow: 0 4px 16px rgba(255, 105, 180, 0.3); /* pink shadow */
  display: flex;
  flex-direction: column;
}

h2 {
  color: #d63384; /* pink fanta */
  text-align: center;
  margin-bottom: 25px;
}

.toggle-show {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
  font-size: 15px;
  color: #6f42c1; /* ungu */
  gap: 8px;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
  max-height: 300px;
  overflow-y: auto;
}

.task-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 0;
  border-bottom: 1px solid #e0b0ff;
}

.task-content {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
}

.done {
  text-decoration: line-through;
  color: #d6a2e8; /* ungu muda */
}

.action-buttons {
  display: flex;
  gap: 8px;
}

.edit-btn,
.delete-btn {
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
}

.edit-btn {
  color: #c2185b; /* pink tua */
}

.delete-btn {
  color: #f44336;
}

.input-group {
  display: flex;
  gap: 10px;
  margin-top: 25px;
  flex-wrap: wrap;
}

input[type="text"] {
  flex: 1;
  padding: 12px;
  border: 1px solid #f8bbd0;
  border-radius: 8px;
  font-size: 15px;
  background-color: #ffe3ec;
  color: #6a1b9a;
}

button {
  padding: 10px 16px;
  font-size: 14px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s;
  background-color: #ec407a;
  color: white;
}

button:hover {
  background-color: #d81b60;
}

.cancel-btn {
  background-color: #f8bbd0;
  color: #4a148c;
}
</style>
