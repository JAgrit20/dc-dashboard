<script>
	import Accordion from '../components/Accordion.svelte'
	import AccordionHeader from '../components/AccordionHeader.svelte'
	import AccordionButton from '../components/AccordionButton.svelte'
	import AccordionBody from '../components/AccordionBody.svelte'

	let queries = []
	let promise = getStudentHelp()

	async function getStudentHelp() {
		const res = await fetch('/api/help-s')
		const data = await res.json()

		if (!res.ok)
			throw new Error('Request to server was successful')

		if (data == null)
			throw new Error('No data received from server.')

		queries = []
		Object.keys(data).forEach(async uid => {
			const student = await (await fetch(`/api/student/${uid}`)).json()
			
			queries = [...queries, {
				student: {
					...student,
					uid,
					hKey: `h-hs--${uid}`,
					cKey: `c-hs--${uid}`
				},
				queryList: data[uid]
			}]
		})

		return true
	}

	const deleteQuery = (uid, qid) => {
		fetch(`/api/help-s?uid=${uid}&qid=${qid}`, {
			method: 'DELETE'
		})
		.then(res => {
			if (res.status === 200) {
				alert('Success, Refresh the page for double confirmation.')
			} else alert('Failure')
		})
		.catch(err => alert(`Failure! Message: ${err.message}`))
		.finally(() => { 
			queries.forEach(qObj => {
				if (qObj.student.uid === uid && qObj.queryList[qid] != undefined) {
					qObj.queryList[qid].confirmDelete = false
					return
				}
			}
		)})
	}

	const refresh = () => { promise = getStudentHelp() }
</script>

<button class="btn btn-secondary" on:click={refresh}>Refresh</button>
{#await promise}
	<p>Requesting . . .</p>
{:then success}
{#if success}
<Accordion id="help-s-accordion">
	{#each queries as queryObj (queryObj.student.uid)}
		<div class="card">
			<AccordionHeader id={queryObj.student.hKey} target={queryObj.student.cKey}>
				{queryObj.student.firstname} {queryObj.student.lastname}
			</AccordionHeader>
			<AccordionBody id={queryObj.student.hKey} target={queryObj.student.cKey} parent="#help-s-accordion">
				<strong>UID: </strong>{queryObj.student.uid}<br>
				<strong>Phone: </strong>{queryObj.student.phonenumber}<br>
				<strong>Email: </strong>{queryObj.student.Email}<br>
				{#each Object.keys(queryObj.queryList) as qId, i (qId)}
					<strong>Query {i+1}: </strong>{queryObj.queryList[qId].query} 
					<span class="badge badge-pill" class:badge-success={queryObj.queryList[qId].closed != true} class:badge-danger={queryObj.queryList[qId].closed}>
						{#if queryObj.queryList[qId].closed}
							CLOSED
						{:else}
							OPEN
						{/if}
					</span>
					<br>
					{#if queryObj.queryList[qId].queryfile}
						<a href={queryObj.queryList[qId].queryfile} target="_blank">Click here to see attachment</a><br>
					{/if}
					{#if !queryObj.queryList[qId].closed}
						<button 
					  	  class="btn btn-primary" 
					  	  on:click={() => queryObj.queryList[qId].confirmDelete = true}
					  	  disabled={queryObj.queryList[qId].confirmDelete}
						>Resolve</button>
					{/if}
					{#if queryObj.queryList[qId].confirmDelete}
						<button
						  class="btn btn-success"
						  on:click={() => deleteQuery(queryObj.student.uid, qId)}
						>Confirm</button>
						<button
						  class="btn btn-danger"
						  on:click={() => queryObj.queryList[qId].confirmDelete = false}
						>Cancel</button>
					{/if}<br><br>
				{/each}
			</AccordionBody>
		</div>
	{/each}
</Accordion>
{:else}
	<p style="color: red;">Error loading data</p>
{/if}
{:catch err}
	<p style="color: red;">{err.message}</p>
{/await}
