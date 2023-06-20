<!-- PokeList.svelte -->
<script>
    // On récupère en propriété le tableau des Pokémons
    // et on lui donne [] comme valeur par défaut.
    export let pokemons = []
  </script>
  
  <ul>
    {#each pokemons as pokemon}
    <li>
      {pokemon.name}
    </li>
    {/each}
  </ul>