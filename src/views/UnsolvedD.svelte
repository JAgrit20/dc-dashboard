<script>
	import Accordion from '../components/Accordion.svelte'
	import AccordionHeader from '../components/AccordionHeader.svelte'
	import AccordionButton from '../components/AccordionButton.svelte'
	import AccordionBody from '../components/AccordionBody.svelte'

	let promise = getUnsolvedD()

	let filterInputDate = ""
	let doubts = []
	async function getUnsolvedD() {
		const res = await fetch('/api/unsolved-d')
		const data = await res.json()

		if (!res.ok)
			throw new Error('Request to server was unsuccessful')

		if (data == null)
			throw new Error('No data received from the server.')

		doubts = []
		Object.keys(data).forEach(async (uid) => {
			const student = await (await fetch(`/api/student/${uid}`)).json()

			if (!filterInputDate) {
				doubts = [...doubts, {
					student: {
						...student,
						uid,
						hKey: `h-ud--${uid}`,
						cKey: `c-ud--${uid}`
					},
					doubtList: data[uid],
				}]
			} else {
				const [year, month, date] = filterInputDate.split("-").map(num => +num)
				const d = {}
				Object.keys(data[uid]).forEach(dId => {
					const doubt = data[uid][dId]
					const [dD, dM, dY] = doubt.date.split("/").map(num => +num)
					if (dD == date && dM == month && dY == year)
						d[dId] = doubt
				})
				if (Object.keys(d) != 0)
					doubts = [...doubts, {
						student: {
							...student,
							uid,
							hKey: `h-ud--${uid}`,
							cKey: `c-ud--${uid}`
						},
						doubtList: d
					}]
			}
		})
		
		return true
	}

	const refresh = () => {
		promise = getUnsolvedD()
	}
</script>

<button class="btn btn-secondary" on:click={refresh}>Refresh</button>
<input type="date" bind:value={filterInputDate}>
{#await promise}
	<p class="text-center">Requesting...</p>
{:then success}
	{#if success}
	<Accordion id="unsolved-d-accordion">
		{#each doubts as doubtObj (doubtObj.student.uid)}
			<div class="card">
				<AccordionHeader id={doubtObj.student.hKey} target={doubtObj.student.cKey}>
					{doubtObj.student.firstname} {doubtObj.student.lastname} has {Object.keys(doubtObj.doubtList).length} doubt(s) unsolved.
				</AccordionHeader> 
				<AccordionBody id={doubtObj.student.hKey} target={doubtObj.student.cKey} parent="#unsolved-d-accordion">
					<strong>UID: </strong>{doubtObj.student.uid}<br>
					<strong>Email: </strong>{doubtObj.student.Email}<br>
					<strong>Phone: </strong>{doubtObj.student.phonenumber}<br>
					<strong>Grade: </strong>{doubtObj.student.Grade}

					<div class="table-responsive mt-3">
						<table class="table table-hover">
							<thead>
								<tr>
									<th scope="col">#</th>
									<th scope="col">Date</th>
									<th scope="col">Subject</th>
									<th scope="col">Topic</th>
								</tr>
							</thead>
							<tbody>
								{#each Object.keys(doubtObj.doubtList) as dId, i (dId)}
									<tr>
										<th scope="row">{i + 1}</th>
										<td>{doubtObj.doubtList[dId].date}</td>
										<td>{doubtObj.doubtList[dId].Subject}</td>
										<td>{doubtObj.doubtList[dId].chapter}</td>
									</tr>
								{/each}
							</tbody>
						</table>
					</div>
				</AccordionBody>
			</div>
		{:else}
			<strong>No Unsolved Doubts pending for this day</strong>
		{/each}		
	</Accordion>
	{:else}
		<p style="color: red;">Error while requesting data from server.</p>
	{/if}
{:catch error}
	<p style="color: red;">{error.message}</p>
{/await}
