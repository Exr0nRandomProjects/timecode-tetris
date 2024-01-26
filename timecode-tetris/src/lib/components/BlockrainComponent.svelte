<script>
import { onMount } from 'svelte';
import { Chart as ChartJS, Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement, } from 'chart.js';
ChartJS.register( Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement);

export let callbacks;

onMount(async () => {
    await import("$lib/blockrain-assets/js/jquery-1.11.1.min.js");
    await Promise.all([
        import("$lib/blockrain-dist/blockrain.jquery.min.js"),
        import("$lib/blockrain-dist/blockrain.css"),
    ]);

    jQuery('.game').blockrain({
        ...callbacks
    });
});


</script>

<div class="game border-2 border-blue-300 h-full aspect-{1/2} m-auto"></div>