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
  import { onMount } from 'svelte';
  import Keypad from './lib/Keypad.svelte';
  import InputField from './lib/inputField.svelte';
  import Timer from './lib/Timer.svelte';
  import Eliza from 'elizabot';
	import { beforeUpdate, afterUpdate } from 'svelte';
  import { tick } from 'svelte';

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

  let html = '<p>Write some text!</p>';

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
  let todos = [
		{ done: false, text: 'finish Svelte tutorial' },
		{ done: false, text: 'build an app' },
		{ done: false, text: 'world domination' }
	];

	function add() {
		todos = todos.concat({ done: false, text: '' });
	}

	function clear() {
		todos = todos.filter((t) => !t.done);
	}

	$: remaining = todos.filter((t) => !t.done).length;
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

  // These values are bound to properties of the video
	let time = 0;
	let duration;
	let paused = true;

	let showControls = true;
	let showControlsTimeout;

	// Used to track time of last mouse down event
	let lastMouseDown;

	function handleMove(e) {
		// Make the controls visible, but fade out after
		// 2.5 seconds of inactivity
		clearTimeout(showControlsTimeout);
		showControlsTimeout = setTimeout(() => (showControls = false), 2500);
		showControls = true;

		if (!duration) return; // video not loaded yet
		if (e.type !== 'touchmove' && !(e.buttons & 1)) return; // mouse not down

		const clientX = e.type === 'touchmove' ? e.touches[0].clientX : e.clientX;
		const { left, right } = this.getBoundingClientRect();
		time = (duration * (clientX - left)) / (right - left);
	}

	// we can't rely on the built-in click event, because it fires
	// after a drag — we have to listen for clicks ourselves
	function handleMousedown(e: any) {
		lastMouseDown = new Date();
	}

	function handleMouseup(e) {
		if (new Date() - lastMouseDown < 300) {
			if (paused) e.target.play();
			else e.target.pause();
		}
	}

	function format(seconds) {
		if (isNaN(seconds)) return '...';

		const minutes = Math.floor(seconds / 60);
		seconds = Math.floor(seconds % 60);
		if (seconds < 10) seconds = '0' + seconds;

		return `${minutes}:${seconds}`;
	}

  let w;
	let h;
	let size = 42;
	let text = 'edit me';

  let canvas;

	onMount(() => {
		const ctx = canvas.getContext('2d');
		let frame = requestAnimationFrame(loop);

		function loop(t) {
			frame = requestAnimationFrame(loop);

			const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);

			for (let p = 0; p < imageData.data.length; p += 4) {
				const i = p / 4;
				const x = i % canvas.width;
				const y = (i / canvas.width) >>> 0;

				const r = 64 + (128 * x) / canvas.width + 64 * Math.sin(t / 1000);
				const g = 64 + (128 * y) / canvas.height + 64 * Math.cos(t / 1000);
				const b = 128;

				imageData.data[p + 0] = r;
				imageData.data[p + 1] = g;
				imageData.data[p + 2] = b;
				imageData.data[p + 3] = 255;
			}

			ctx.putImageData(imageData, 0, 0);
		}

		return () => {
			cancelAnimationFrame(frame);
		};
	});

  let pin;
	$: view = pin ? pin.replace(/\d(?!$)/g, '•') : 'enter your pin';

	function handleSubmitPin() {
		alert(`submitted ${pin}`);
	}

  let field;


	let photos = [];

  onMount(async () => {
    const res = await fetch(`/tutoriel/api/album`);
    console.log("cnx:", res)
    photos = await res.json();
    console.log("photos:", photos)
  });

  let open = false;
	let seconds = 0;

	const toggleTimer = () => (open = !open);
	const handleTick = () => (seconds += 1);

  let div;
	let autoscroll;

	beforeUpdate(() => {
		autoscroll = div && div.offsetHeight + div.scrollTop > div.scrollHeight - 20;
	});

	afterUpdate(() => {
		if (autoscroll) div.scrollTo(0, div.scrollHeight);
	});

	const eliza = new Eliza();

	let comments = [{ author: 'eliza', text: eliza.getInitial() }];

	function handleKeydown(event) {
		if (event.key === 'Enter') {
			const text = event.target.value;
			if (!text) return;

			comments = comments.concat({
				author: 'user',
				text
			});

			event.target.value = '';

			const reply = eliza.transform(text);

			setTimeout(() => {
				comments = comments.concat({
					author: 'eliza',
					text: '...',
					placeholder: true
				});

				setTimeout(() => {
					comments = comments
						.filter((comment) => !comment.placeholder)
						.concat({
							author: 'eliza',
							text: reply
						});
				}, 500 + Math.random() * 500);
			}, 200 + Math.random() * 200);
		}
	}

  let textTick = `Select some text and hit the tab key to toggle uppercase`;

  async function handleKeydownTick(event:any) {
    if (event.key !== 'Tab') return;

    event.preventDefault();

    const { selectionStart, selectionEnd, value } = this;
    const selection = value.slice(selectionStart, selectionEnd);

    const replacement = /[a-z]/.test(selection) ? selection.toUpperCase() : selection.toLowerCase();

    textTick = value.slice(0, selectionStart) + replacement + value.slice(selectionEnd);

    await tick();
    this.selectionStart = selectionStart;
    this.selectionEnd = selectionEnd;
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


  <h3>Bindings/Select multiple</h3>
  <p>Size</p>
  <label>
    <input type="radio" bind:group={scoops} value={1} />
    One scoop
  </label>

  <label>
    <input type="radio" bind:group={scoops} value={2} />
    Two scoops
  </label>

  <label>
    <input type="radio" bind:group={scoops} value={3} />
    Three scoops
  </label>

  <p>Flavours</p>

  <select multiple bind:value={flavours}>
    {#each menu as flavour}
      <option value={flavour}>
        {flavour}
      </option>
    {/each}
  </select>

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


  <h3>Bindings/Contenteditable</h3>
  <div contenteditable="true" bind:innerHTML={html} />
  <pre>{html}</pre>



  <h3>Bindings/Each block bindings</h3>
  {#each todos as todo}
	<div class:done={todo.done}>
		<input type="checkbox" bind:checked={todo.done} />
		<input placeholder="What needs to be done?" bind:value={todo.text} />
	</div>
  {/each}
  <p>{remaining} remaining</p>
  <button on:click={add}> Add new </button>
  <button on:click={clear}> Clear completed </button>


  <h3>Bindings/Media element</h3>
  <p>Caminandes: Llamigos</p>
  <p>From <a href="https://studio.blender.org/films">Blender Studio</a>. CC-BY</p>

  <div>
    <video
      poster="https://sveltejs.github.io/assets/caminandes-llamigos.jpg"
      src="https://sveltejs.github.io/assets/caminandes-llamigos.mp4"
      on:mousemove={handleMove}
      on:touchmove|preventDefault={handleMove}
      on:mousedown={handleMousedown}
      on:mouseup={handleMouseup}
    >
      <track kind="captions" />
    </video>

    <div class="controls" style="opacity: {duration && showControls ? 1 : 0}">
      <progress value={time / duration || 0} />

      <div class="info">
        <span class="time">{format(time)}</span>
        <span>click anywhere to {paused ? 'play' : 'pause'} / drag to seek</span>
        <span class="time">{format(duration)}</span>
      </div>
    </div>
  </div>



  <h3>Bindings/Dimensions</h3>
   <input type="range" bind:value={size} />
<input bind:value={text} />

  <p>size: {w}px x {h}px</p>

  <div bind:clientWidth={w} bind:clientHeight={h}>
    <span class="dimension" style="font-size: {size}px">{text}</span>
  </div>



  <h3>Bindings/This</h3>
  <canvas bind:this={canvas} width={32} height={32} />


  <h3>Bindings/PIN</h3>
  <p style="color: {pin ? '#333' : '#ccc'}">{view}</p>
  <Keypad bind:value={pin} on:submit={handleSubmitPin} />


  <h3>Bindings/Binding to component instances</h3>
  <InputField bind:this={field} />
  <button on:click={() => field.focus()}>Focus field</button>


  <h3>Lifecycle/onMount</h3>
  <div class="photos">
    {#each photos as photo}
      <figure>
        <img src={photo.thumbnailUrl} alt={photo.title} />
        <figcaption>{photo.title}</figcaption>
      </figure>
    {:else}
      <!-- this block renders when photos.length === 0 -->
      <p>loading...</p>
    {/each}
  </div>


  <h3>Lifecycle/Timer</h3>
  <div>
    <button on:click={toggleTimer}>{open ? 'Close' : 'Open'} Timer</button>
    <p>
      The Timer component has been open for
      {seconds}
      {seconds === 1 ? 'second' : 'seconds'}
    </p>
    {#if open}
      <Timer callback={handleTick} />
    {/if}
  </div>


  <h3>Lifecycle/beforeUpdate and afterUpdate</h3>
  <div class="scrollable" bind:this={div}>
		{#each comments as comment}
			<article class={comment.author}>
				<span class="title-main">{comment.text}</span>
			</article>
		{/each}
	</div>
	<input on:keydown={handleKeydown} />


  <h3>Lifecycle/tick</h3>
  <textarea value={textTick} on:keydown={handleKeydownTick} />



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
  .dimension{
    color:rgb(10, 25, 48);
  }
  .done {
		opacity: 0.4;
	}

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

  [contenteditable] {
		padding: 0.5em;
		border: 1px solid #eee;
		border-radius: 4px;
	}

  .controls {
		position: absolute;
		top: 0;
		width: 100%;
		transition: opacity 1s;
	}

	.info {
		display: flex;
		width: 100%;
		justify-content: space-between;
	}

	span {
		padding: 0.2em 0.5em;
		color: white;
		text-shadow: 0 0 8px black;
		font-size: 1.4em;
		opacity: 0.7;
	}

	.time {
		width: 3em;
	}

	.time:last-child {
		text-align: right;
	}

	progress {
		display: block;
		width: 100%;
		height: 10px;
		-webkit-appearance: none;
		appearance: none;
	}

	progress::-webkit-progress-bar {
		background-color: rgba(0, 0, 0, 0.2);
	}

	progress::-webkit-progress-value {
		background-color: rgba(255, 255, 255, 0.6);
	}

	video {
		width: 100%;
	}
  
  canvas {
		/* width: 100%;
		height: 100%; */
		background-color: #666;
		/* -webkit-mask: url(/svelte-logo-mask.svg) 50% 50% no-repeat;
		mask: url(/svelte-logo-mask.svg) 50% 50% no-repeat; */
	}

  .photos {
		width: 100%;
		display: grid;
		grid-template-columns: repeat(5, 1fr);
		grid-gap: 8px;
	}

	figure,
	img {
		width: 100%;
		margin: 0;
	}
  textarea {
		width: 100%;
		height: 200px;
	}
</style>
