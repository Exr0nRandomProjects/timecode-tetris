<script>
    import Blockrain from "$lib/components/BlockrainComponent.svelte";
    let charts_shown = true;
</script>

<body class="overscroll-none h-screen w-screen fixed flex justify-center space-x-12">
    <div class="h-screen w-1/3">
        <Blockrain />
    </div>
    <div class="flex-col">
        <div class="">
            <span class="items-center">
                <input type="checkbox" bind:checked={charts_shown} class="toggle block" />
                Show Graphs
            </span>
            {#if charts_shown}
                <div class="border-1- border-red-100 w-[30rem] h-full">
                    hi
                </div>
            {/if}
        </div>
    </div>
</body>