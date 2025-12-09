<template>
  <div id="app-container">
    <div class="card">
      <header>
        <h1>Fun Translator 💬✨</h1>
        <p class="subtitle">Transforme seus textos em linguagens épicas!</p>
      </header>

      <div class="controls">
        <label for="type-select">Escolha o estilo:</label>
        <div class="select-wrapper">
          <select id="type-select" v-model="translationType">
            <option v-for="t in types" :key="t" :value="t">
              {{ capitalize(t) }}
            </option>
          </select>
        </div>
      </div>

      <textarea 
        v-model="input" 
        placeholder="Digite seu texto aqui (em inglês funciona melhor)..."
      ></textarea>

      <button @click="translate" class="btn-translate">
        Traduzir Agora 🚀
      </button>

      <div v-if="output" class="result-box">
        <h3>Resultado:</h3>
        <blockquote>{{ output }}</blockquote>
      </div>

      <div v-if="error" class="error-box">
        ⚠️ {{ error }}
      </div>
    </div>
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
    // Pequeno helper para deixar a primeira letra maiúscula no select
    capitalize(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
    async translate() {
      this.output = '';
      this.error = '';

      if (!this.input.trim()) {
        this.error = "Por favor, digite algo antes de traduzir! 💀";
        return;
      }

      try {
        const res = await fetch(`https://api.funtranslations.com/translate/${this.translationType}.json?text=${encodeURIComponent(this.input)}`);
        const data = await res.json();

        if (data.error) {
          // A API gratuita tem limites estritos (5 requests/hora)
          this.error = "Ops! " + data.error.message;
          return;
        }

        this.output = data.contents.translated;
      } catch (e) {
        this.error = "Erro de conexão 😭 (Verifique sua internet ou tente mais tarde)";
      }
    }
  }
};
</script>

<style>
/* Reset Global e Fontes */
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap');

body {
  margin: 0;
  font-family: 'Poppins', sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #333;
}

/* Container Principal */
#app-container {
  width: 100%;
  display: flex;
  justify-content: center;
  padding: 20px;
}

/* O Cartão (Card) */
.card {
  background: #ffffff;
  width: 100%;
  max-width: 500px;
  padding: 2.5rem;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
  text-align: center;
  transition: transform 0.3s ease;
}

.card:hover {
  transform: translateY(-5px);
}

/* Tipografia */
h1 {
  margin-top: 0;
  margin-bottom: 0.5rem;
  font-size: 2rem;
  color: #4a4a4a;
}

.subtitle {
  color: #777;
  font-size: 0.9rem;
  margin-bottom: 2rem;
}

/* Controles de Formulário */
.controls {
  text-align: left;
  margin-bottom: 1rem;
}

label {
  font-weight: 600;
  font-size: 0.9rem;
  color: #555;
  display: block;
  margin-bottom: 0.5rem;
}

.select-wrapper select {
  width: 100%;
  padding: 12px;
  border: 2px solid #e0e0e0;
  border-radius: 10px;
  background-color: #f9f9f9;
  font-size: 1rem;
  font-family: inherit;
  cursor: pointer;
  outline: none;
  transition: border-color 0.3s;
}

.select-wrapper select:focus {
  border-color: #764ba2;
}

textarea {
  width: 100%;
  height: 120px;
  padding: 15px;
  border: 2px solid #e0e0e0;
  border-radius: 12px;
  background-color: #f9f9f9;
  font-family: inherit;
  font-size: 1rem;
  resize: vertical;
  box-sizing: border-box; /* Garante que o padding não estoure a largura */
  outline: none;
  transition: all 0.3s;
}

textarea:focus {
  border-color: #764ba2;
  background-color: #fff;
  box-shadow: 0 4px 10px rgba(118, 75, 162, 0.1);
}

/* Botão */
.btn-translate {
  width: 100%;
  padding: 15px;
  margin-top: 1.5rem;
  background: linear-gradient(to right, #667eea, #764ba2);
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
}

.btn-translate:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(118, 75, 162, 0.4);
}

.btn-translate:active {
  transform: translateY(0);
}

/* Área de Resultado */
.result-box {
  margin-top: 2rem;
  background: #f0f4f8;
  border-left: 5px solid #667eea;
  padding: 1rem;
  border-radius: 8px;
  text-align: left;
  animation: fadeIn 0.5s ease-in-out;
}

.result-box h3 {
  margin: 0 0 0.5rem 0;
  font-size: 1rem;
  color: #555;
}

blockquote {
  margin: 0;
  font-size: 1.1rem;
  font-style: italic;
  color: #333;
}

/* Erro */
.error-box {
  margin-top: 1.5rem;
  padding: 10px;
  background-color: #ffe6e6;
  color: #d8000c;
  border-radius: 8px;
  border: 1px solid #ffb3b3;
  font-size: 0.9rem;
  animation: shake 0.5s;
}

/* Animações */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes shake {
  0% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  50% { transform: translateX(5px); }
  75% { transform: translateX(-5px); }
  100% { transform: translateX(0); }
}
</style>