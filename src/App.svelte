<script>
	import PhaseTwo from './views/PhaseTwo.svelte'
	import OnlineT from './views/OnlineT.svelte'
	import Students from './views/Students.svelte'
	import HelpS from './views/HelpS.svelte'
	import HelpT from './views/HelpT.svelte'
	import TicketS from './views/TicketS.svelte'
	import TicketT from './views/TicketT.svelte'
	import RequestEdits from './views/RequestEdits.svelte'
	import UnsolvedD from './views/UnsolvedD.svelte'
	import IncomeT from './views/IncomeT.svelte'

	const optionList = [
		{ value: 'p2', content: 'View teachers in phase 2'},
		{ value: 'ot', content: 'View currently online teachers'},
		{ value: 'st', content: 'View students currently signed up'},
		{ value: 'hs', content: 'View students seeking for help'},
		{ value: 'ht', content: 'View teachers seeking for help'},
		{ value: 'ts', content: 'View tickets raised by students'},
		{ value: 'tt', content: 'View tickets raised by teachers'},
		{ value: 're', content: 'View teachers requesting edits'},
		{ value: 'ud', content: 'View unsolved doubts by date'},
		{ value: 'it', content: 'Income for the teachers'}
	]
	let selected = optionList[0].value
</script>

<h1 class="text-center font-weight-bold m-3">Dashboard</h1>
<label for="type">Select an option:  </label>
<select id="type" name="type" bind:value={selected}>
	{#each optionList as item}
		<option value={item.value}>{item.content}</option>
	{/each}
</select>

{#if selected === 'p2'}
  	<PhaseTwo />
{:else if selected === 'ot'}
	<OnlineT />
{:else if selected === 'st'}
	<Students />
{:else if selected === 'hs'}
  	<HelpS />
{:else if selected === 'ht'}
  	<HelpT />
{:else if selected === 'ts'}
  	<TicketS />
{:else if selected === 'tt'}
  	<TicketT />
{:else if selected === 're'}
  	<RequestEdits />
{:else if selected === 'ud'}
  	<UnsolvedD />
{:else if selected === 'it'}
  	<IncomeT />
{:else}
  	<div class="text-center mt-4 text-uppercase font-weight-bold font-italic">
		You have not selected any option
	</div>
{/if}
