<script lang="ts">
  import svelteLogo from './assets/svelte.svg';
  import viteLogo from '/vite.svg';
  import Counter from './lib/Counter.svelte';
  import PokeList from './lib/PokeList.svelte';
  import Nested from './lib/Nested.svelte';

  let name = 'world'

   // La variable qui va stocker notre promesse
   const promise = getPokemons();

  // La fonction qui va récupérer les Pokémons
  async function getPokemons() {
    const res = await fetch('https://pokeapi.co/api/v2/pokemon');
    const json = await res.json();

    return json.results;
  }
</script>

<main>
  <div>
    <a href="https://vitejs.dev" target="_blank" rel="noreferrer">
      <img src={viteLogo} class="logo" alt="Vite Logo" />
    </a>
    <a href="https://svelte.dev" target="_blank" rel="noreferrer">
      <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
    </a>
  </div>
  <h1>Vite + Svelte</h1>
  <p class="title-main ">Hello {name.toUpperCase()}!</p>

  <div class="card">
    <Counter />
  </div>

  <p>
    Check out <a href="https://github.com/sveltejs/kit#readme" target="_blank" rel="noreferrer">SvelteKit</a>, the official Svelte app framework powered by Vite!
  </p>

  <p class="read-the-docs">
    Click on the Vite and Svelte logos to learn more
  
  <h3>props/declaring props/default values</h3>
  <p><Nested answer={'42'}/></p>
  <Nested/>


  <h1>Mini projet Pokédex</h1>

  <!--
    `#await` va permettre de gérer la promesse
    directement dans le corps du composant
  -->
  {#await promise}
    Chargement du Pokédex
  <!-- Une fois résolue, on a le retour de notre fonction `GetPokemons` -->
  {:then pokemons}
  <PokeList pokemons={pokemons} />
  <!-- Sinon, en cas d'erreur, on affiche un message -->
  {:catch error}
    Une erreur s'est produite : {error.message}
  {/await}


</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
  .title-main {
		color: purple;
		font-family: 'Comic Sans MS', cursive;
		font-size: 2em;
	}
</style>
