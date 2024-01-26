<script>
    import { Scatter } from 'svelte-chartjs';

    import Blockrain from "$lib/components/BlockrainComponent.svelte";
    let charts_shown = true;


    const charts_tracks = {
        keydown:     { backgroundColor: "blue" },
        keyup:       { backgroundColor: "yellow" },

        Placed:      { backgroundColor: "lightgreen" },
        Line:        { backgroundColor: "darkgreen" },
    };

    const make_default_scatter_data = () => {
        const key_map = Object.fromEntries(Object.keys(charts_tracks).map((k, i) => [k, i]));
        const data = {
            datasets: Object.entries(charts_tracks).map(([k, v], i) => ({
                label: k,
                data: [],
                ...v
            }))
        };
        // data.pushPoint = (key, point) => { data.datasets[key_map[key]]?.data.push(point); }
        data.pushPoint = (key, y) => { data.datasets[key_map[key]]?.data.push({ x: Date.now() - start_time, y }); }
        return data;
    }


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


    const KECODE_TO_CHART_Y = {
        ArrowLeft:  2,
        ArrowRight: 3,
        ArrowUp:    4,
        ArrowDown:  5,
    }

    const callbacks = {
        onStart: () => {
            console.log("callback called!")
            start_time = Date.now();
            scatter_data = make_default_scatter_data()
        },

        onLine: () => {
            scatter_data.pushPoint('Line', 0);
        },
        onPlaced: () => {
            scatter_data.pushPoint('Placed', 1);
        },

        onKey: (evt) => {
            if (evt.key in KECODE_TO_CHART_Y) {
                scatter_data.pushPoint(evt.type, KECODE_TO_CHART_Y[evt.key]);
            }
        },

        onTick: () => {
            chart_options.scales.x.max = Date.now() - start_time;
            chart?.update();
        },
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