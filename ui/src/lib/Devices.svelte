<script lang="ts">
	import { config, devices, showAll } from '$lib/stores';
	import { zoomIdentity } from 'd3-zoom';

	import DeviceDot from './DeviceDot.svelte';
	import type { Device } from './types';

	export let transform = zoomIdentity;
	export let floorId: string | null = null;
	export let deviceId: string | null = null;

	const filter = (devices: Device[]) => {
		if (deviceId != null) return devices.filter((d) => d.id === deviceId);
		if (showAll) return devices.filter((d) => d.floor?.id === floorId);
		return devices.filter((d) => d.floor?.id === floorId && d.id !== deviceId);
	};
</script>

<g transform={transform.toString()}>
	{#if $devices}
		{#each filter($devices) as d (d.id)}
			{#if d.confidence > 1 && d.location}
				<DeviceDot {d} on:hovered on:selected />
			{/if}
		{/each}
	{/if}
</g>
