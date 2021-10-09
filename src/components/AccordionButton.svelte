<script>
	export let target
</script>

<button class="accordion-button" type="button" data-bs-toggle="collapse" data-bs-target={`#${target}`} aria-expanded="false" aria-controls={target}>
	<slot></slot>
</button>
