<script>
    import { Scatter } from 'svelte-chartjs';

    import Blockrain from "$lib/components/BlockrainComponent.svelte";
    let charts_shown = true;


    const callback_names = ['Key', 'BlockAppear', 'Placed', 'Line'];

    $: scatter_data = {
        datasets: callback_names.map((k, i) => ({
            label: k,
            data: [],
        }))
    }

    let chart;
    const event_callback = (idx, time) => {
        scatter_data.datasets[idx]?.data?.push({ x: time, y: idx });
        chart.update();
    }
</script>

<body class="overscroll-none h-screen w-screen fixed flex justify-center space-x-12">
    <div class="h-screen w-1/3">
        <Blockrain event_callback={event_callback} callback_names={callback_names} />
    </div>
    <div class="flex-col">
        <div class="">
            <span class="items-center">
                <input type="checkbox" bind:checked={charts_shown} class="toggle block" />
                Show Graphs
            </span>
            {#if charts_shown}
                <div class="border-1- border-red-100 w-[30rem] h-full">
                    <Scatter bind:chart data={scatter_data} options={{ responsive: true }} />
                </div>
            {/if}
        </div>
    </div>
</body>