<script lang="ts">
	import Simulator from './Simulator.svelte';
	import PopupMenu from './PopupMenu.svelte';
	import Edit from './Edit.svelte';
	import type { EmsCall } from '$lib/emsCall';
	import { Pencil, Plus } from '@lucide/svelte';
	import { onMount } from 'svelte';
	import { AppBar } from '@skeletonlabs/skeleton-svelte';

	let simActive = $state(false);
	let calls: EmsCall[] = $state([]);
	let activeCall: EmsCall | undefined = $state();
	let editMode = $state(false);
	let editActive = $state(false);

	onMount(() => {
		restoreEmsCallsFromLocalStorage();
	});

	$effect(() => {
		saveEmsCallsToLocalStorage(calls);
	});

	function open(call: EmsCall) {
		activeCall = call;
		if (editMode) {
			editActive = true;
		} else {
			simActive = true;
		}
	}

	function createNewEmsCall() {
		activeCall = {
			name: 'Neuer Einsatz',
			code: '',
			codeDescr: '',
			descr: '',
			patient: '',
			dataSource: '',
			location: '',
			sentCar: '',
			callText: ''
		};
		calls.push(activeCall);
		editActive = true;
	}

	const LOCAL_STORAGE_KEY = 'emsCalls';

	function restoreEmsCallsFromLocalStorage() {
		if (window.localStorage.getItem(LOCAL_STORAGE_KEY) !== null) {
			calls.push(...JSON.parse(window.localStorage[LOCAL_STORAGE_KEY]));
		}
	}

	function saveEmsCallsToLocalStorage(emsCalls: EmsCall[]) {
		window.localStorage[LOCAL_STORAGE_KEY] = JSON.stringify(emsCalls);
	}
</script>

{#if simActive && activeCall}
	<Simulator emsCall={activeCall} bind:simActive></Simulator>
{:else if editActive && activeCall}
	<Edit bind:activeCall bind:editActive bind:calls></Edit>
{:else}
	<AppBar
		base="sticky top-0"
		background={editMode ? 'preset-filled-primary-100-900' : 'preset-filled-primary-500'}
	>
		{#snippet lead()}
			{#if !editMode}
				<PopupMenu bind:calls></PopupMenu>
			{/if}
		{/snippet}
		{#snippet trail()}
			<button onclick={() => (editMode = !editMode)}>
				<Pencil class="size-5" />
			</button>
		{/snippet}
		{#if editMode}
			<h1>EDIT MODE</h1>
		{:else}
			<h1>ES App Faker</h1>
		{/if}
	</AppBar>
	{#if calls.length === 0}
		<div class="mt-4 flex items-center justify-center">Keine Alarmierungen</div>
	{/if}
	{#each calls as call (call.name)}
		<button class="w-full p-4 text-left" onclick={() => open(call)}>{call.name}</button>
		<hr class="hr" />
	{/each}
	{#if editMode}
		<button class="btn preset-filled-primary-500 float-right mt-2" onclick={createNewEmsCall}>
			<Plus />
			Einsatz hinzufügen
		</button>
	{/if}
{/if}
