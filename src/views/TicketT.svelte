<script>
	import Accordion from '../components/Accordion.svelte'
	import AccordionHeader from '../components/AccordionHeader.svelte'
	import AccordionButton from '../components/AccordionButton.svelte'
	import AccordionBody from '../components/AccordionBody.svelte'

	let tickets = []
	let promise = getTeachersTicket()

	async function getTeachersTicket() {
		const res = await fetch('/api/ticket-t')
		const data = await res.json()

		if (!res.ok)
			throw new Error('Request to server was unsuccessful')

		if (data == null)
			throw new Error('Empty data has been received from the server')
		
		tickets = []
		Object.keys(data).forEach(async (uid) => {
			const teacher = await (await fetch(`/api/teacher/${uid}`)).json()
			tickets = [...tickets, {
				teacher: {
					...teacher,
					uid,
					hKey: `h-tt--${uid}`,
					cKey: `c-tt--${uid}`
				},
				ticketList: data[uid]
			}]
		})

		return true
	}

	const deleteQuery = (uid, qid) => {
		fetch(`/api/ticket-t?uid=${uid}&qid=${qid}`, {
			method: 'DELETE'
		})
		.then(res => {
			if (res.status === 200) {
				alert('Success, Refresh the page for double confirmation.')
			} else alert('Failure')
		})
		.catch(err => alert(`Failure! Message: ${err.message}`))
		.finally(() => {
			tickets.forEach(ticket => {
				if (ticket.teacher.uid === uid && ticket.ticketList[qid] != undefined) {
					ticket.ticketList[qid].confirmDelete = false
					return
				}
			})
		})
	}

	const refresh = () => { promise = getTeachersTicket() }
</script>

<button class="btn btn-secondary" on:click={refresh}>Refresh</button>
{#await promise}
	<p>Requesting. . .</p>
{:then success}
{#if success}
<Accordion id="ticket-t-accordion">
	{#each tickets as ticketObj (ticketObj.teacher.uid)}
		<div class="card">
			<AccordionHeader id={ticketObj.teacher.hKey} target={ticketObj.teacher.cKey}>
				{ticketObj.teacher.firstname} {ticketObj.teacher.lastname}
			</AccordionHeader>
			<AccordionBody id={ticketObj.teacher.hKey} target={ticketObj.teacher.cKey} parent="#ticket-t-accordion">
				<strong>UID: </strong>{ticketObj.teacher.uid}<br>
				<strong>Phone: </strong>{ticketObj.teacher.phone}<br>
				<strong>Email: </strong>{ticketObj.teacher.Email}<br>
				{#each Object.keys(ticketObj.ticketList) as qId, i (qId)}
					<strong>Query {i+1}: </strong>{ticketObj.ticketList[qId].query}
					<span class="badge badge-pill" class:badge-success={ticketObj.ticketList[qId].closed != true} class:badge-danger={ticketObj.ticketList[qId].closed}>
						{#if ticketObj.ticketList[qId].closed}
							CLOSED
						{:else}
							OPEN
						{/if}
					</span>
					<br>
					{#if ticketObj.ticketList[qId].queryfile}
						<a href={ticketObj.ticketList[qId].queryfile} target="_blank">Click here to see attachment</a><br>
					{/if}
					{#if ticketObj.ticketList[qId].closed}
					<button 
					  class="btn btn-primary" 
					  on:click={() => ticketObj.ticketList[qId].confirmDelete = true}
					  disabled={ticketObj.ticketList[qId].confirmDelete}
					>Resolve</button>
					{/if}
					{#if ticketObj.ticketList[qId].confirmDelete}
						<button
						  class="btn btn-success"
						  on:click={() => deleteQuery(ticketObj.teacher.uid, qId)}
						>Confirm</button>
						<button
						  class="btn btn-danger"
						  on:click={() => ticketObj.ticketList[qId].confirmDelete = false}
						>Cancel</button>
					{/if}
					<br><br>
				{/each}
			</AccordionBody>
		</div>
	{/each}
</Accordion>
{:else}
	<p style="color: red;">Error while requesting data</p>
{/if}
{:catch err}
	<p style="color: red;">{err.message}</p>
{/await}
