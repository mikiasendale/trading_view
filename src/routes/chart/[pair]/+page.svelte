<script lang="ts">
    import { page } from '$app/stores';
    import TradingViewAdvancedChart from '$lib/TradingViewAdvancedChart.svelte';

    // Mapping from URL parameter to TradingView symbol
    const symbolMap: Record<string, string> = {
        eurusd: "CMCMARKETS:EURUSD",
        gbpusd: "CMCMARKETS:GBPUSD",
        gbpjpy: "CMCMARKETS:GBPJPY",
        gold: "CAPITALCOM:GOLD",
        usdjpy: "CMCMARKETS:USDJPY",
        us100: "CAPITALCOM:US100"
        // Add more pairs here as needed
    };

    // Reactive statement to get the pair from the URL
    $: pair = $page.params.pair?.toLowerCase(); // Use lowercase for case-insensitive matching

    // Reactive statement to get the corresponding symbol
    $: symbol = pair ? symbolMap[pair] : undefined;

</script>

{#if symbol}
    <!-- Display the pair name in the heading -->
    <h1>{pair?.toUpperCase()} Chart</h1>
    <!-- Pass the resolved symbol to the component -->
    <TradingViewAdvancedChart {symbol} />
{:else if pair}
    <!-- Handle cases where the pair exists but is not in our map -->
    <h1>Invalid Pair</h1>
    <p>Sorry, the trading pair "{pair}" is not supported.</p>
{:else}
    <!-- Handle cases where pair parameter might be missing (shouldn't usually happen with this route structure) -->
    <h1>Error</h1>
    <p>Could not determine trading pair from URL.</p>
{/if}

<style>
    h1 {
        text-align: center;
        margin: 1rem 0;
        text-transform: uppercase;
    }
    p {
        text-align: center;
    }
</style> 