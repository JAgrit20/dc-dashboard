<script>
	import Accordion from '../components/Accordion.svelte'
	import AccordionHeader from '../components/AccordionHeader.svelte'
	import AccordionButton from '../components/AccordionButton.svelte'
	import AccordionBody from '../components/AccordionBody.svelte'

	let teachers = []
	let promise = getRequestEditList()

	async function getRequestEditList() {
		const res = await fetch('/api/request-edits')
		const data = await res.json()

		if (!res.ok)
			throw new Error('Request to server was successful.')

		if (data == null)
			throw new Error('No data received from the server.')

		teachers = []
		Object.keys(data).forEach(uid => {
			teachers = [...teachers, {
				...data[uid],
				uid,
				hKey: `h-re--${uid}`,
				cKey: `c-re--${uid}`,
				confirmChange: false
			}]
		})

		return true
	}

	const putRequestAccess = uid => {
		fetch(`/api/request-edits?uid=${uid}`, {
			method: 'PUT'
		}).then(res => {
			if (res.status === 200) {
				alert('Success, Refresh the page for double confirmation.')
			} else alert('Failure')
		})
		.catch(err => alert(`Failure! Message: ${err.message}`))
	}


	const refresh = () => { promise = getRequestEditList() }
</script>

<button class="btn btn-secondary" on:click={refresh}>Refresh</button>
{#await promise}
	<p>Requesting . . .</p>
{:then success}
{#if success}
<Accordion id="request-e-accordion">
	{#each teachers as teacher (teacher.uid)}
		<div class="card">
			<AccordionHeader id={teacher.hKey} target={teacher.cKey}>
				{teacher.firstname} {teacher.lastname} - Online
			</AccordionHeader>
			<AccordionBody id={teacher.hKey} target={teacher.cKey} parent="#request-e-accordion">
				<strong>UID: </strong>{teacher.uid}<br>
				<strong>Name: </strong>{teacher.firstname} {teacher.lastname}<br>
				<strong>Email: </strong>{teacher.Email}<br>
				<strong>Phone Number: </strong>{teacher.phone}<br>
				<strong>D.O.B: </strong>{teacher.dob}<br>
				<strong>Rate per doubt: </strong>{teacher.rate_per_doubt}<br>
				<strong>Primary Language: </strong>{teacher.lang}<br>
				<strong>Tags: </strong><br>
				<ul>
					{#each teacher.tags.split(", ") as tag (tag)}
						<li>{tag}</li>
					{/each}				
				</ul>
				
				<button 
				  class="btn btn-primary m-3 px-1 py-2" 
				  disabled={teacher.confirmChange} 
				  on:click={() => teacher.confirmChange = true}
				>Give Change access</button>
				{#if teacher.confirmChange}
					<button 
					  class="btn btn-success m-3 px-1 py-2"
					  on:click={() => {
					  	teacher.confirmChange = false
						putRequestAccess(teacher.uid)
					  }}
					>Confirm</button>
					<button
					  class="btn btn-danger m-3 px-1 py-2"
					  on:click={() => teacher.confirmChange = false}
					>Cancel</button>
				{/if}
			</AccordionBody>
		</div>
	{:else}
		<strong class="text-center">No Teacher is online currently</strong>
	{/each}
</Accordion>
{:else}
	<p style="color: red;">Error while requesting data.</p>
{/if}
{:catch err}
	<p style="color: red;">{err.message}</p>
{/await}
