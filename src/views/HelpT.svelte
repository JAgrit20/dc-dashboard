<script>
	import Accordion from '../components/Accordion.svelte'
	import AccordionHeader from '../components/AccordionHeader.svelte'
	import AccordionButton from '../components/AccordionButton.svelte'
	import AccordionBody from '../components/AccordionBody.svelte'

	let queries = []
	let promise = getTeachersHelp()

	async function getTeachersHelp() {
		const res = await fetch('/api/help-t')
		const data = await res.json()

		if (!res.ok)
			throw new Error('Request to server was unsuccessful')

		if (data == null)
			throw new Error('Empty data has been received from the server')
		
		queries = []
		Object.keys(data).forEach(async (uid) => {
			const teacher = await (await fetch(`/api/teacher/${uid}`)).json()
			queries = [...queries, {
				teacher: {
					...teacher,
					uid,
					hKey: `h-ht--${uid}`,
					cKey: `c-ht--${uid}`
				},
				queryList: response[uid]
			}]
		})

		return true
	}

	const deleteQuery = (uid, qid) => {
		fetch(`/api/help-t?uid=${uid}&qid=${qid}`, {
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
				if (qObj.teacher.uid === uid && qObj.queryList[qid] != undefined) {
					qObj.queryList[qid].confirmDelete = false
					return
				}
			})
		})
	}

	const refresh = () => { promise = getTeachersHelp() }
</script>

<button class="btn btn-secondary" on:click={refresh}>Refresh</button>
{#await promise}
	<p>Requesting. . .</p>
{:then success}
{#if success}
<Accordion id="help-t-accordion">
	{#each queries as queryObj (queryObj.teacher.uid)}
		<div class="card">
			<AccordionHeader id={queryObj.teacher.hKey} target={queryObj.teacher.cKey}>
				{queryObj.teacher.firstname} {queryObj.teacher.lastname}
			</AccordionHeader>
			<AccordionBody id={queryObj.teacher.hKey} target={queryObj.teacher.cKey} parent="#help-t-accordion">
				<strong>UID: </strong>{queryObj.teacher.uid}<br>
				<strong>Phone: </strong>{queryObj.teacher.phone}<br>
				<strong>Email: </strong>{queryObj.teacher.Email}<br>
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
						  on:click={() => deleteQuery(queryObj.teacher.uid, qId)}
						>Confirm</button>
						<button
						  class="btn btn-danger"
						  on:click={() => queryObj.queryList[qId].confirmDelete = false}
						>Confirm</button>
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
