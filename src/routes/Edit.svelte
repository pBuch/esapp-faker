<script lang="ts">
	import type { EmsCall } from '$lib/emsCall';

	let {
		activeCall = $bindable(),
		editActive = $bindable(),
		calls = $bindable()
	}: {
		activeCall: EmsCall;
		editActive: boolean;
		calls: EmsCall[];
	} = $props();

	function saveActive(event: SubmitEvent & { currentTarget: EventTarget & HTMLFormElement }) {
		event.preventDefault();
		const data = new FormData(event.currentTarget);
		activeCall!.name = data.get('name') as string;
		activeCall!.code = data.get('code') as string;
		activeCall!.codeDescr = data.get('codeDescr') as string;
		activeCall!.descr = data.get('descr') as string;
		activeCall!.patient = data.get('patient') as string;
		activeCall!.dataSource = data.get('dataSource') as string;
		activeCall!.location = data.get('location') as string;
		activeCall!.sentCar = data.get('sentCar') as string;
		activeCall!.callText = data.get('callText') as string;
		editActive = false;
	}

	function deleteActive() {
		const idx = calls.findIndex((call) => call.name === activeCall!.name);
		calls.splice(idx, 1);
		editActive = false;
	}
</script>

<form onsubmit={saveActive} class="space-y-2 py-4">
	<label class="label">
		<span class="label-text">Einsatzname</span>
		<input name="name" class="input" type="text" value={activeCall.name} />
	</label>
	<label class="label">
		<span class="label-text">Einsatzcode</span>
		<input name="code" class="input" type="text" value={activeCall.code} />
	</label>
	<label class="label">
		<span class="label-text">Einsatzcode-Wortlaut</span>
		<input name="codeDescr" class="input" type="text" value={activeCall.codeDescr} />
	</label>
	<label class="label">
		<span class="label-text">Beschreibung</span>
		<input name="descr" class="input" type="text" value={activeCall.descr} />
	</label>
	<label class="label">
		<span class="label-text">Patient</span>
		<input name="patient" class="input" type="text" value={activeCall.patient} />
	</label>
	<label class="label">
		<span class="label-text">Funkgruppe</span>
		<input name="dataSource" class="input" type="text" value={activeCall.dataSource} />
	</label>
	<label class="label">
		<span class="label-text">Einsatzort</span>
		<textarea class="textarea" name="location" rows="4">{activeCall.location}</textarea>
	</label>
	<label class="label">
		<span class="label-text">Alarmiertes Einsatzmittel</span>
		<input name="sentCar" class="input" type="text" value={activeCall.sentCar} />
	</label>
	<label class="label">
		<span class="label-text">Alarmierungsprotokoll</span>
		<textarea class="textarea" name="callText" rows="6">{activeCall.callText}</textarea>
	</label>
	<div class="flex flex-row">
		<button class="btn preset-outlined-primary-500 w-1/2" onclick={() => (editActive = false)}
			>Abbrechen</button
		>
		<button class="btn preset-filled-primary-500 w-1/2" type="submit">Speichern</button>
	</div>
</form>
<button class="btn preset-tonal-error w-1/2" onclick={deleteActive}>Einsatz löschen</button>
