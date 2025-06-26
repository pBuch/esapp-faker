<script lang="ts">
	import { Popover, FileUpload } from '@skeletonlabs/skeleton-svelte';
	import { Menu, Upload, Download } from '@lucide/svelte';
	import type { EmsCall } from '$lib/emsCall';
	import type { FileAcceptDetails } from '@zag-js/file-upload';

	let { calls = $bindable() }: { calls: EmsCall[] } = $props();
	let exportActive = $state(false);
	let exportHref = $state('');

	function menuOpenChange(open: boolean) {
		if (open) {
			exportCalls();
		} else {
			cleanExportCalls();
		}
		exportActive = open;
	}

	function exportCalls() {
		const data = JSON.stringify(calls);
		const blob = new Blob([data], { type: 'application/json' });
		exportHref = URL.createObjectURL(blob);
	}

	function cleanExportCalls() {
		URL.revokeObjectURL(exportHref);
	}

	function importEmsCalls(uploadedFiles: FileAcceptDetails) {
		const uploadedFile = uploadedFiles.files[0];
		const reader = new FileReader();
		reader.onload = () => {
			const emsCalls: EmsCall[] = JSON.parse(reader.result as string);
			calls.splice(0, calls.length);
			calls.push(...emsCalls);
		};
		reader.readAsText(uploadedFile);
		menuOpenChange(false);
	}
</script>

<Popover
	open={exportActive}
	onOpenChange={(e) => menuOpenChange(e.open)}
	positioning={{ placement: 'bottom' }}
	contentBase="card bg-surface-200-800 p-4 space-y-4"
	arrow
	arrowBackground="!bg-surface-200"
>
	{#snippet trigger()}
		<Menu class="size-5" />
	{/snippet}
	{#snippet content()}
		<div class="flex flex-col">
			<FileUpload
				name="example-button"
				accept="application/json"
				onFileAccept={importEmsCalls}
				maxFiles={1}
			>
				<div class="flex flex-row items-center gap-2 p-2">
					<Upload class="size-4" />
					<span>Import</span>
				</div>
			</FileUpload>

			<a
				id="my-export-link"
				class="flex flex-row items-center gap-2 p-2"
				download="esapp-faker-export.json"
				href={exportHref}
			>
				<Download class="size-4" />
				Export
			</a>
		</div>
	{/snippet}
</Popover>
