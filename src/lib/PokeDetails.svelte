<!-- PokeDetails.svelte -->
<script>
    export let pokemon
    let promise
  
    // Pour ré-exécuter du code après le changement d’une variable
    // il faut ajouter `$:` devant.
    // Ici la promesse sera ré-exécuté lorsque la variable `pokemon` changera
    // Les blocks `$:` ne sont exécuté que lorsque qu’une variable présente
    // dans le bloc change
    $: promise = getPokemon(pokemon.url)
  
    async function getPokemon(url) {
      const res = await fetch(url);
      return await res.json();
    }
  
  </script>
  
  <div>
  <!-- On va réutiliser `await` pour le traitement de la promesse -->
    {#await promise}
      Chargement de {pokemon.name}…
    {:then pokemonDetails}
      <h2>{pokemonDetails.name}</h2>
      <img src={pokemonDetails.sprites.front_default} alt={pokemonDetails.name} />
    {:catch error}
      Une erreur s'est produite : {error.message}
    {/await}
  </div>