<script>
    import { Scatter } from 'svelte-chartjs';

    import Blockrain from "$lib/components/BlockrainComponent.svelte";
    let charts_shown = true;


    const callback_charts = {
        Key:         { backgroundColor: "blue" },
        Line:        { backgroundColor: "darkgreen" },
        BlockAppear: { backgroundColor: "darkgreen" },
        Placed:      { backgroundColor: "lightgreen" },
    };

    const make_default_scatter_data = () => ({
        datasets: Object.entries(callback_charts).map(([k, v], i) => ({
            label: k,
            data: [],
            ...v
        }))
    });

    $: scatter_data = make_default_scatter_data();
    $: chart_options = {
        responsive: true,
        scales: {
            x: {
                min: 0,
                max: 0,
            }
        }
    }

    let chart;
    let start_time = Date.now();


    const chart_callbacks = Object.fromEntries(Object.keys(callback_charts).map((k, i) =>
        ['on' + k, (...args) => {
            scatter_data.datasets[i]?.data?.push({ x: Date.now()-start_time, y: i });
        }]
    ))


    const callbacks = {
        onStart: () => {
            console.log("callback called!")
            start_time = Date.now();
            scatter_data = make_default_scatter_data()
        },
        onTick: () => {
            chart_options.scales.x.max = Date.now() - start_time;
            chart.update();
        },
        ...chart_callbacks
    }
</script>

<body class="overscroll-none h-screen w-screen fixed flex justify-center space-x-12">
    <div class="h-screen w-1/3">
        <Blockrain {callbacks} />
    </div>
    <div class="flex-col">
        <div class="">
            <span class="items-center">
                <input type="checkbox" bind:checked={charts_shown} class="toggle block" />
                Show Graphs
            </span>
            {#if charts_shown}
                <div class="border-1- border-red-100 w-[30rem] h-full">
                    <Scatter bind:chart data={scatter_data} options={chart_options} />
                </div>
            {/if}
        </div>
    </div>
</body>