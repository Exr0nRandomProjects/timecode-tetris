<script>
import { onMount } from 'svelte';
import { Chart as ChartJS, Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement, } from 'chart.js';
ChartJS.register( Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement);

export let callback_names;
export let event_callback;

onMount(async () => {
    await import("$lib/blockrain-assets/js/jquery-1.11.1.min.js");
    await Promise.all([
        import("$lib/blockrain-dist/blockrain.jquery.min.js"),
        import("$lib/blockrain-dist/blockrain.css"),
    ]);

    const callbacks = Object.fromEntries(callback_names.map((k, i) =>
        ['on' + k, () => {
            // scatter_data[i]?.data?.push(Date.now());
            event_callback(i, Date.now());
        }]
    ))

    jQuery('.game').blockrain({
        ...callbacks
    });
});


</script>

<div class="game border-2 border-blue-300 h-full aspect-{1/2} m-auto"></div>