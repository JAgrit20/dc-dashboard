<script>
	export let target
	export let id
	export let parent
</script>

<div id={target} class="collapse" aria-labelledby={id} data-parent={parent}>
	<div class="card-body">
		<slot></slot>
	</div>
</div>
