<script>
	export let id
	export let target
</script>

<div class="card-header" {id}>
	<h2 class="mb-0">
		<button
		  class="btn btn-link btn-block text-left"
		  type="button"
		  data-toggle="collapse"
                  data-target={`#${target}`}
		  aria-expanded="false"
		  aria-controls={target}
		>
			<slot></slot>
		</button>
	</h2>
</div> 
