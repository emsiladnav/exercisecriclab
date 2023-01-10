<script lang="ts">
    export let title: string;
    export let icon: any;
</script>

<li class={`flex my-2 cursor-pointer items-center whitespace-nowrap ${title != 'Data Mapping' ? 'space-x-2' : 'space-x-3'}`}>
    <img src="icons/{icon}.svg" alt="{title}" class={title != 'Data Mapping' ? 'w-6' : 'ml-1 w-4'} />
    <p class="font-medium text-sm leading-snug">
        {title}
    </p>
</li>
