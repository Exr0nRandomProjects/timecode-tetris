<script>
import { onMount } from 'svelte';
import { Scatter } from 'svelte-chartjs';
import { Chart as ChartJS, Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement, } from 'chart.js';
ChartJS.register( Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement);

let moments = {
    key: [],
    blockappear: [],
    placed: [],
    line: []
}

onMount(async () => {
    await import("$lib/blockrain-assets/js/jquery-1.11.1.min.js");
    await Promise.all([
        import("$lib/blockrain-dist/blockrain.jquery.min.js"),
        import("$lib/blockrain-dist/blockrain.css"),
    ]);

    jQuery('.game').blockrain({
        onKey: () => { moments.key.push(Date.now()) },
        onBlockAppear: () => { moments.blockappear.push(Date.now()) },
        placed: () => { moments.placed.push(Date.now()) },
        line: () => { moments.line.push(Date.now()) },
    });
});

const scatter_data = {
    datasets: Object.entries(moments).map(([k, v], i) => ({
        label: k,
        data: v.map(t => ({ x: t, y: i }))
    }))
}
</script>

<div class="game border-2 border-blue-300 h-full aspect-{1/2} m-auto"></div>
<Scatter data={scatter_data} options={{ responsive: true }} />