<script>
import { onMount } from 'svelte';

onMount(async () => {
    const jq = await import("$lib/blockrain-assets/js/jquery-1.11.1.min.js");
    await import("$lib/blockrain-dist/blockrain.jquery.min.js");
    await import("$lib/blockrain-dist/blockrain.css");

    jQuery('.game').blockrain({ onKey: console.log, onBlockAppear: console.log });
})
</script>

<div class="game border-2 border-blue-300 h-full aspect-[1/2] m-auto"></div>