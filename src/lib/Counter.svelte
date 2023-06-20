<script lang="ts">
  let count: number = 0;
  let numbers = [1, 2, 3, 4];

  $: doubled = count * 2  //reactive declaration
  $: if (count >= 10) {
		alert('count is dangerously high!');
		count = 9;
	}
  $: {
	console.log('the count is ' + count);
  console.log('sum is ' + sum);
	//alert('I SAID THE COUNT IS ' + count);
  }
  $: sum = numbers.reduce((t, n) => t + n, 0);
  
  const increment = () => {
    count += 1;
    numbers = [...numbers, numbers.length + 1];
  }
  // const addNumber = () => {
  //   numbers.push(numbers.length + 1);
  // }

</script>

<button on:click={increment}>
  count is {count} {count === 1 ? 'time' : 'times'}
</button>

<p>{count} doubled is {doubled}</p>
<p>{numbers.join(' + ')} = {sum}</p>
