<script lang="ts">
	export let title: string;
	export let color: string | null = 'bg-white';
	export let textColor: string | null = 'text-black';
	export let icon: any | null = null;
	export let outline: boolean | null = true;
	export let outlineColor: string | null = 'border-black';
	export let onClick: any | null = null;
</script>

<button
	class={`${color && color} ${
		outline == true ? `border ${outlineColor} border-opacity-20` : null
	} rounded-md hover:opacity-80 cursor-pointer`}
	on:click={onClick}
>
	<div class="flex space-x-2 items-center justify-start h-full px-3 py-3">
		{#if icon}
			<img src={`icons/${icon}.svg`} alt={icon} />
		{/if}
		<p class={`text-sm font-medium leading-snug ${textColor && textColor}`}>{title}</p>
	</div>
</button>
 