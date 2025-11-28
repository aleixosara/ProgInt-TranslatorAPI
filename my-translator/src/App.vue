<template>
  <div id="app">
    <h1>Fun Translator 💬✨</h1>

    <select v-model="translationType">
      <option v-for="t in types" :key="t" :value="t">{{ t }}</option>
    </select>

    <textarea v-model="input" placeholder="Digite o texto"></textarea>

    <button @click="translate">Traduzir</button>

    <p v-if="output"><strong>Resultado:</strong> {{ output }}</p>
    <p v-if="error" style="color:red">{{ error }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      input: '',
      output: '',
      error: '',
      translationType: 'yoda',
      types: [
        "yoda", "pirate", "shakespeare", "valyrian",
        "dolan", "gungan", "chef", "vulcan", "minion"
      ]
    };
  },
  methods: {
    async translate() {
      this.output = '';
      this.error = '';

      if (!this.input.trim()) {
        this.error = "Digite algo antes 💀";
        return;
      }

      try {
        const res = await fetch(`https://api.funtranslations.com/translate/${this.translationType}.json?text=${encodeURIComponent(this.input)}`);
        const data = await res.json();

        if (data.error) {
          this.error = data.error.message;
          return;
        }

        this.output = data.contents.translated;
      } catch (e) {
        this.error = "Erro ao conectar 😭 (CORS ou limite da API)";
      }
    }
  }
};
</script>

<style>
#app {
  max-width: 600px;
  margin: 2rem auto;
  text-align: center;
}
textarea {
  width: 100%;
  height: 120px;
  margin: 1rem 0;
  padding: 10px;
  border-radius: 8px;
}
select, button {
  padding: 10px;
  margin: 0.5rem;
  border-radius: 8px;
}
button {
  cursor: pointer;
}
</style>
