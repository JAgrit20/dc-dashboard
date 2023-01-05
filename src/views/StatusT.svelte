<script>
	import Accordion from '../components/Accordion.svelte'
	import AccordionHeader from '../components/AccordionHeader.svelte'
	import AccordionButton from '../components/AccordionButton.svelte'
	import AccordionBody from '../components/AccordionBody.svelte'

	let teachers = []
	let promise = getTeachersEng()

	async function getTeachersEng() {
		const res = await fetch('/api/engadeged-teachers')
		const data = await res.json()

		if (!res.ok)
			throw new Error('Request to server was unsucessful')

		if (data == null)
			throw new Error('No data received from servers.')

		teachers = []
		Object.keys(data).forEach(uid => {
			teachers = [...teachers, {
				...data[uid],
				uid,
				hKey: `h-it--${uid}`,
				cKey: `c-it--${uid}`
			}]
		})

		return true
	}
	
	const refresh = () => { promise = getTeachersEng() }
</script>

<button class="btn btn-secondary" on:click={refresh}>Refresh</button>
{#await promise}
	<p>Requesting . . . </p>
{:then success}
<Accordion id="income-t-accordion">  
	{#each teachers as teacher (teacher.uid)}
		<div class="card">
			<AccordionHeader id={teacher.hKey} target={teacher.cKey}>
				{teacher.firstname} {teacher.lastname}
			</AccordionHeader>
			<AccordionBody id={teacher.hKey} target={teacher.cKey} parent="#income-t-accordion">
				<strong>Test: </strong>{teacher.uid}<br>
				<strong>Name: </strong>{teacher.firstname} {teacher.lastname}<br>
				<strong>Email: </strong>{teacher.Email}<br>
				<strong>Phone Number: </strong>{teacher.phone}<br>
				<strong>Primary Language: </strong>{teacher.lang}<br>
				<strong>Status: </strong>{teacher.status}<br>
				
			</AccordionBody>
		</div>
	{:else}
		<strong class="text-center">No Teacher is online currently</strong>
	{/each}
</Accordion>
{:catch err}
	<p style="color: red;">{err.message}</p>
{/await} 
