<script lang="ts">
  import svelteLogo from './assets/svelte.svg';
  import viteLogo from '/vite.svg';
  import Counter from './lib/Counter.svelte';
  import PokeList from './lib/PokeList.svelte';
  import PokeDetails from './lib/PokeDetails.svelte';
  import Nested from './lib/Nested.svelte';
  import Info from './lib/Info.svelte';
  import Thing from './lib/Thing.svelte';
  import Inner from './lib/Inner.svelte';
  import Outer from './lib/Outer.svelte';
  import CustomButton from './lib/CustomButton.svelte';
  import {marked} from 'marked';

  let name = 'world';
  let user = { loggedIn: false };
  let user2 = { loggedIn: false };
  let x = 7;
  let cats = [
		{ id: 'J---aiyznGQ', name: 'Keyboard Cat' },
		{ id: 'z_AbfPXTKms', name: 'Maru' },
		{ id: 'OUtn3pvWmpg', name: 'Henri The Existential Cat' }
	];
  let things = [
		{ id: 1, name: 'apple' },
		{ id: 2, name: 'banana' },
		{ id: 3, name: 'carrot' },
		{ id: 4, name: 'doughnut' },
		{ id: 5, name: 'egg' }
	];
  let title='world';
  let a = 1;
	let b = 2;
  let yes = false;

  let scoops = 1;
	let flavours = ['Mint choc chip'];

  let value = `Some words are *italic*, some are **bold**`;

  function join(flavours) {
		if (flavours.length === 1) return flavours[0];
		return `${flavours.slice(0, -1).join(', ')} and ${flavours[flavours.length - 1]}`;
	}

	let menu = ['Cookies and cream', 'Mint choc chip', 'Raspberry ripple'];

	function handleClick() {
		things = things.slice(1);
	}
  async function getRandomNumber() {
		//const res = await fetch(randomRange(1,100));
    // const res = setTimeout(() => {
    //   randomRange(1,2);
    // }, 200);
    // console.log("rest:", res);
  
    const res = randomRange(1,100)
  //  console.log("rest:" + res);
		//const text = await res.text();
   // console.log("text:" + text);

		if (res) {
      console.log("Success!!!")
			return res;     
		} else {
			throw new Error("erreur!!!!");
		}
	}

  // function generateRandomNumberAfterDelay(delay: number) {
  //   return setTimeout(function() {
  //     const randomNumber = randomRange(1,100);
  //     console.log("Nombre aléatoire généré :", randomNumber);
  //   }, delay);
  //  }

  // Exemple d'utilisation avec un délai de 3 secondes (3000 millisecondes)
  //generateRandomNumberAfterDelay(3000);

  let promiseRandom = getRandomNumber();
  

  function handleClickRandom() {
    promiseRandom = getRandomNumber();
    console.log("promiseRandom :",promiseRandom)
  }

  function randomRange(myMin: number, myMax: number) {
  return (Math.floor(Math.random() * (myMax - myMin + 1)) + myMin);
  }


  let pokemons = []
  // On initialise la variable qui va contenir le Pokémon choisi
  let selectedPokemon = null

  const handlePokemonSelect = (event) => {
    // Les données spécifique envoyé dans l'évènement seront
    // dans la propriété `detail`
    selectedPokemon = event.detail
  }

   // La variable qui va stocker notre promesse
   const promise = getPokemons();

  // La fonction qui va récupérer les Pokémons
  async function getPokemons() {
    const res = await fetch('https://pokeapi.co/api/v2/pokemon');
    const json = await res.json();

    return json.results;
  }

  function toggle() {
		user.loggedIn = !user.loggedIn;
	}

  function toggle2() {
		user2.loggedIn = !user2.loggedIn;
	}
  const pkg = {
		name: 'svelte',
		version: 3,
		speed: 'blazing',
		website: 'https://svelte.dev'
	};

  let m = { x: 0, y: 0 };
  let questions = [
		{ id: 1, text: `Where did you go to school?` },
		{ id: 2, text: `What is your mother's name?` },
		{ id: 3, text: `What is another personal fact that an attacker could easily find with Google?` }
	];

	let selected;

	let answer = '';

  function handleSubmitBinding() {
		alert(`answered question ${selected.id} (${selected.text}) with "${answer}"`);
	}

  function handleMousemove(event: { clientX: number; clientY: number; }) {
    m.x = event.clientX;
    m.y = event.clientY;
  }

  function handleClickAlert() {
		alert('no more alerts');
	}

  function handleMessage(event) {
		alert(event.detail.text);
	}

  function handleClickButton() {
		alert('Button Clicked');
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
  
  <h3>props/declaring props/default values/spreads props</h3>
  <p><Nested answer={'42'}/></p>
  <Nested/>
  <Info {...pkg}/>

  <h3>logic/if block</h3>
  {#if user.loggedIn}
    <button on:click={toggle}>
      Log out
    </button>
  {/if}

  {#if !user.loggedIn}
    <button on:click={toggle}>
      Log in
    </button>
  {/if}

  <h3>logic/else blocks</h3>
  {#if user2.loggedIn}
	<button on:click={toggle2}>
		Log out
	</button>
  {:else}
    <button on:click={toggle2}>
      Log in
    </button>
  {/if}

  <h3>logic/else-if blocks</h3>
  {#if x > 10}
	<p>{x} is greater than 10</p>
  {:else if 5 > x}
    <p>{x} is less than 5</p>
  {:else}
    <p>{x} is between 5 and 10</p>
  {/if}

  <h3>logic/each blocks</h3>
  <ul>
    {#each cats as { id, name }, i}
      <li><a target="_blank" href="https://www.youtube.com/watch?v={id}" rel="noreferrer">
        {i + 1}: {name}
      </a></li>
    {/each}
  </ul>

  <h3>logic/Keyed each blocks</h3>
  <button on:click={handleClick}> Remove first thing </button>
  {#each things as thing (thing.id)}
    <Thing name={thing.name} />
  {/each}

  <h3>logic/Await blocks</h3>
  <button on:click={handleClickRandom}> generate random number </button>
  <!-- replace this element -->
  {#await promiseRandom}
	<p>...waiting</p>
  {:then number}
    <p>The number is {number}</p>
  {:catch error}
    <p style="color: red">{error.message}</p>
  {/await}

  <h3>Events/DOM event</h3>
  <div on:mousemove={handleMousemove}>
    The mouse position is {m.x} x {m.y}
  </div>

  <h3>Events/Inline handlers</h3>
  <div on:mousemove={(e) => (m = { x: e.clientX, y: e.clientY })}>
    The mouse position is {m.x} x {m.y}
  </div>

  <h3>Events/Event modifiers</h3>
  <button on:click|once={handleClickAlert}> Cliquer moi </button>

  <h3>Events/Component event</h3>
  <Inner on:message={handleMessage} />

  <h3>Events/Event forwarding</h3>
  <Outer on:message={handleMessage} />

  <h3>Events/DOM event forwarding</h3>
  <CustomButton on:click={handleClickButton} />

  <h3>Bindings/Text inputs</h3>
  <input bind:value={title} />
  <p>Hello {title}!<p>

  <h3>Bindings/Numerics inputs</h3>
  <label>
    <input type="number" bind:value={a} min="0" max="10" />
    <input type="range" bind:value={a} min="0" max="10" />
  </label>
  
  <label>
    <input type="number" bind:value={b} min="0" max="10" />
    <input type="range" bind:value={b} min="0" max="10" />
  </label>
  
  <p>{a} + {b} = {a + b}</p>

  <h3>Bindings/Checkbox inputs</h3>
  <label>
    <input type="checkbox" bind:checked={yes} />
    Yes! Send me regular email spam
  </label>
  
  {#if yes}
    <p>Thank you. We will bombard your inbox and sell your personal details.</p>
  {:else}
    <p>You must opt-in to continue. If you're not paying, you're the product.</p>
  {/if}  
  <button disabled={!yes}> Subscribe </button>


  <h3>Bindings/Group inputs</h3>
  <p>Size</p>
  <label>
    <input type="radio" bind:group={scoops} name="scoops" value={1} />
    One scoop
  </label>
  <label>
    <input type="radio" bind:group={scoops} name="scoops" value={2} />
    Two scoops
  </label>
  <label>
    <input type="radio" bind:group={scoops} name="scoops" value={3} />
    Three scoops
  </label>
  <p>Flavours</p>
  {#each menu as flavour}
    <label>
      <input type="checkbox" bind:group={flavours} name="flavours" value={flavour} />
      {flavour}
    </label>
  {/each}

  {#if flavours.length === 0}
    <p>Please select at least one flavour</p>
  {:else if flavours.length > scoops}
    <p>Can't order more flavours than scoops!</p>
  {:else}
    <p>
      You ordered {scoops}
      {scoops === 1 ? 'scoop' : 'scoops'}
      of {join(flavours)}
    </p>
  {/if}
  
  <h3>Bindings/Textarea inputs</h3>
  {@html marked(value)}
  <textarea bind:value />

  <h3>Bindings/Select bindings</h3>
  <p>Insecurity questions</p>
  <form on:submit|preventDefault={handleSubmitBinding}>
    <select bind:value={selected} on:change={() => (answer = '')}>
      {#each questions as question}
        <option value={question}>
          {question.text}
        </option>
      {/each}
    </select>
	<input bind:value={answer} />
	<button disabled={!answer} type="submit"> Submit </button>
  </form>
  <p>selected question {selected ? selected.id : '[waiting...]'}</p>

  <h1>Pokédex</h1>
  <!-- {#await promise}
  Chargement du Pokédex
{:then pokemons}
  <PokeList pokemons={pokemons} on:pokemonSelect="{handlePokemonSelect}" /> -->
  <!-- Le nom du Pokémon sélectionné est affiché -->
  <!-- {selectedPokemon}
{:catch error}
  Une erreur s'est produite : {error.message}
{/await} -->
{#await promise}
  Chargement du Pokédex...
{:then pokemons}
  <PokeList pokemons={pokemons} on:selectPokemon={selectedPokemon}  />


<!-- Si on a séléctionné un Pokémon, on peut afficher notre composant -->
  {#if selectedPokemon}
  <PokeDetails pokemon={selectedPokemon} />
    
<!-- Sinon, une indication -->
  {:else}
    Sélectionnez un Pokémon 
  {/if}
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
  h1 {
    text-align: center;
  }
  div {
    display: flex;
  }
</style>
