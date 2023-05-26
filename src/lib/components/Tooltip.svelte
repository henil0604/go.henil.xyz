<script lang="ts">
	import random from '$lib/module/random';
	import { popup, type PopupSettings } from '@skeletonlabs/skeleton';

	export let text = 'Tooltip text';
	export let placement = 'bottom';
	export let event = 'hover';
	export let variant = 'filled-primary';

	const id = random(20);

	let popupHover = {
		event,
		target: `popup-${id}`,
		placement
	} as PopupSettings;
</script>

<div>
	<div class="card p-2 variant-{variant}" data-popup="popup-{id}">
		<p>{text}</p>
		<slot name="tooltip-content" />
		<div class="arrow variant-{variant}" />
	</div>

	<div use:popup={popupHover}>
		<slot />
	</div>
</div>
