<script lang="ts">
	import { ChevronRight, Menu, RadioTower, Settings, Undo2 } from '@lucide/svelte';
	import type { EmsCall } from '$lib/emsCall';

	let { emsCall, simActive = $bindable() }: { emsCall: EmsCall; simActive: boolean } = $props();

	function closeSim() {
		simActive = false;
	}
</script>

<header class="preset-filled-primary-500 sticky top-0 flex flex-row justify-between p-4">
	<button onclick={closeSim}>
		<Menu />
	</button>
	<h1>ES App Faker</h1>
	<RadioTower />
</header>

<div class="flex flex-col gap-4 p-4">
	<div>
		<b>{emsCall.code}</b> Einsatz <b>{emsCall.codeDescr}</b>
		{emsCall.descr}
	</div>

	<hr class="hr" />

	<div class="flex flex-row justify-between">
		<span><b>Patient</b></span>
		<span class="text-surface-300"
			>{emsCall.patient}
			<ChevronRight class="inline" /></span
		>
	</div>

	<hr class="hr" />

	<div class="flex flex-row justify-between">
		<span><b>Daten</b></span>
		<span class="text-surface-300"
			>{emsCall.dataSource}
			<ChevronRight class="inline" /></span
		>
	</div>

	<hr class="hr" />

	<div class="flex flex-row items-center justify-between">
		<div class="flex flex-col">
			<b>Einsatzort</b>
			<span class="text-surface-300 whitespace-pre-line">{emsCall.location}</span>
		</div>
		<ChevronRight class="text-surface-300 inline" />
	</div>

	<hr class="hr" />

	<div class="flex flex-row items-center justify-between">
		<div class="flex flex-col">
			<b>Zielort</b>
			<span class="text-surface-300">
				-<br />
				<br />
			</span>
		</div>
		<ChevronRight class="text-surface-300 inline" />
	</div>

	<hr class="hr" />

	<div class="flex flex-row items-center justify-between">
		<div class="flex flex-col">
			<b>Alarmiert</b>
			<span class="text-surface-300">{emsCall.sentCar}</span>
		</div>
		<ChevronRight class="text-surface-300 inline" />
	</div>

	<hr class="hr" />

	<div class="whitespace-pre-line">
		{emsCall.callText}
	</div>

	<div class="h-12"></div>
</div>

<footer
	class="preset-filled-surface-400-600 fixed bottom-0 flex w-full max-w-[30rem] flex-row pt-2 pb-2"
>
	<div class="flex w-1/2 flex-col items-center gap-1">
		<Settings />
		Aktion
	</div>
	<button onclick={closeSim} class="flex w-1/2 flex-col items-center gap-1">
		<Undo2 />
		Events
	</button>
</footer>
