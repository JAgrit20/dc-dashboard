<script>
	import Accordion from '../components/Accordion.svelte'
	import AccordionHeader from '../components/AccordionHeader.svelte'
	import AccordionButton from '../components/AccordionButton.svelte'
	import AccordionBody from '../components/AccordionBody.svelte'

	let students = []
	let promise = getStudents()

	async function getStudents() {
		const res = await fetch('/api/signed-students')
		const data = await res.json()

		if (!res.ok)
			throw new Error('Request to server was unsucessful')

		if (data == null)
			throw new Error('No data received from server.')

		students = []
		Object.keys(data).forEach(uid => {
			students = [...students, {
				...data[uid],
				uid,
				hKey: `h-st--${uid}`,
				cKey: `c-st--${uid}`
			}]
		})

		return true
	}
	
	const refresh = () => { promise = getStudents() }
</script>

<button class="btn btn-secondary" on:click={refresh}>Refresh</button>
{#await promise}
	<p>Requesting . . . </p>
{:then success}
<Accordion id="students-s-accordion">
	{#each students as student (student.uid)}
		<div class="card">
			<AccordionHeader id={student.hKey} target={student.cKey}>
				{student.firstname} {student.lastname}
			</AccordionHeader>
			<AccordionBody id={student.hKey} target={student.cKey} parent="#students-s-accordion">
				<strong>UID: </strong>{student.uid}<br>
				<strong>Name: </strong>{student.firstname} {student.lastname}<br>
				<strong>Email: </strong>{student.Email}<br>
				<strong>Phone Number: </strong>{student.phonenumber}<br>
				<strong>Primary Language: </strong>{student.lang}<br>
				<strong>Board/Grade: </strong>{student.Board} - {student.Grade}
			</AccordionBody>
		</div>
	{:else}
		<strong class="text-center">No Teacher is online currently</strong>
	{/each}
</Accordion>
{:catch err}
	<p style="color: red;">{err.message}</p>
{/await} 
