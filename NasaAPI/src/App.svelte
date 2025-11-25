<script>
  let input = "";
  let output = "";
  let error = "";

  async function translate() {
    output = "";
    error = "";

    try {
      // IMPORTANTE: GET COM TEXTO NA URL
      const url =
        "https://api.funtranslations.com/translate/yoda.json?text=" +
        encodeURIComponent(input);

      const res = await fetch(url);

      const data = await res.json();

      if (data.error) {
        error = data.error.message;
        return;
      }

      output = data.contents.translated;
    } catch (e) {
      error = "Erro ao conectar 😭 (provavelmente CORS)";
    }
  }
</script>

<h1>Tradutor Yoda</h1>

<input bind:value={input} placeholder="Digite algo" />
<button on:click={translate}>Traduzir</button>

{#if output}
  <p><strong>Resultado:</strong> {output}</p>
{/if}

{#if error}
  <p style="color:red;">{error}</p>
{/if}
