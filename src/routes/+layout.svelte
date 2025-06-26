<script lang="ts">
	import '../app.css';

	let { children } = $props();
</script>

<div class="h-full max-w-[30rem]">
	{@render children()}
</div>
