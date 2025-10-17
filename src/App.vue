<template>
  <div class="contenidor">
    <h2>Exercici Learn English</h2>
    <p class="autor">Robinbir Singh</p>

    <p class="punts">Punts: {{ punts }} / {{ actuals.length }}</p>

    <div class="taula">
      <!-- Columna paraules -->
      <div class="col">
        <button
          v-for="(paraula, i) in catala"
          :key="i"
          @click="comprova(paraula)"
          :disabled="encerts.includes(paraula)"
          class="boto text"
        >
          {{ paraula }}
        </button>
      </div>

      <!-- Columna àudios -->
      <div class="col">
        <button
          v-for="(item, i) in actuals"
          :key="i"
          @click="parla(item.en)"
          :disabled="encerts.includes(item.en)"
          class="boto so"
        >
          🔊
        </button>
      </div>
    </div>

    <button class="reinicia" @click="reinicia">Torna a començar</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      totes: [
        { en: 'cat', ca: 'gat' },
        { en: 'banana', ca: 'plàtan' },
        { en: 'tree', ca: 'arbre' },
        { en: 'blue', ca: 'blau' },
        { en: 'pencil', ca: 'llapis' },
        { en: 'bicycle', ca: 'bicicleta' },
        { en: 'table', ca: 'taula' },
        { en: 'moon', ca: 'lluna' },
        { en: 'small', ca: 'petit' },
        { en: 'flower', ca: 'flor' },
        { en: 'fish', ca: 'peix' },
        { en: 'white', ca: 'blanc' },
        { en: 'hospital', ca: 'hospital' },
        { en: 'sing', ca: 'cantar' },
      ],
      actuals: [],
      catala: [],
      actual: null,
      punts: 0,
      encerts: [],
    }
  },
  methods: {
    barreja(arr) {
      return arr.sort(() => Math.random() - 0.5)
    },
    reinicia() {
      this.actuals = this.barreja(this.totes).slice(0, 6)
      this.catala = this.barreja(this.actuals.map((x) => x.ca))
      this.punts = 0
      this.encerts = []
      this.actual = null
    },
    parla(text) {
      let veu = speechSynthesis.getVoices().find((v) => v.lang.includes('en')) || null
      let frase = new SpeechSynthesisUtterance(text)
      frase.voice = veu
      speechSynthesis.speak(frase)
      this.actual = this.actuals.find((x) => x.en === text)
    },
    comprova(paraula) {
      if (!this.actual) return
      if (this.actual.ca === paraula) {
        this.punts++
        this.encerts.push(paraula, this.actual.en)
      }
      this.actual = null
    },
  },
  mounted() {
    this.reinicia()
  },
}
</script>

<style scoped>
.contenidor {
  width: 300px;
  text-align: center;
  font-family: sans-serif;
}

h2 {
  background: linear-gradient(135deg, #667eea, #764ba2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-size: 26px;
  font-weight: 800;
  letter-spacing: 1px;
}

p {
  color: #666;
}

.taula {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 15px;
  margin-top: 20px;
}

.col {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.boto {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border: 2px solid #5a6dd8;
  border-radius: 8px;
  cursor: pointer;
  height: 55px;
  transition: all 0.3s ease;
  font-size: 18px;
  color: white;
  font-weight: 600;
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
}
.boto:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(102, 126, 234, 0.5);
  background: linear-gradient(135deg, #764ba2 0%, #667eea 100%);
}
button:disabled {
  opacity: 0.5;
}

.punts {
  font-weight: bold;
  font-size: 16px;
  color: #667eea;
  background: transparent;
  padding: 8px 0;
}

.reinicia {
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  border: 2px solid #f5576c;
  border-radius: 6px;
  color: white;
  cursor: pointer;
  margin-top: 15px;
  padding: 8px 15px;
  font-weight: 600;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(245, 87, 108, 0.3);
}
.reinicia:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(245, 87, 108, 0.5);
  background: linear-gradient(135deg, #f5576c 0%, #f093fb 100%);
}

.autor {
  text-align: center;
  color: #999;
  font-size: 14px;
  margin-top: 5px;
  font-style: italic;
}
</style>
