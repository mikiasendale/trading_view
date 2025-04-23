<script lang="ts">
  import { onMount } from 'svelte';

  let widgetContainer: HTMLDivElement | null = null; // Reference to the inner container div
  export let symbol: string; // Accept symbol as a prop

  onMount(() => {
    const script = document.createElement('script');
    script.type = 'text/javascript';
    script.src = 'https://s3.tradingview.com/external-embedding/embed-widget-advanced-chart.js';
    script.async = true;
    // Convert the configuration object to a JSON string for the script content
    script.textContent = JSON.stringify({
      "autosize": true,
      "symbol": symbol,
      "interval": "60",
      "timezone": "Etc/UTC",
      "theme": "dark",
      "style": "1",
      "locale": "en",
      "allow_symbol_change": false,
      "studies": [
        "STD;Bollinger_Bands",
        "STD;On_Balance_Volume",
        "STD;Divergence%1Indicator"
      ],
      "support_host": "https://www.tradingview.com"
    });

    // Append the script to the specific widget container div after the component mounts
    if (widgetContainer) {
      // Clear the container first in case of re-renders, though unlikely needed with onMount
      // widgetContainer.innerHTML = '';
      widgetContainer.appendChild(script);
    }

    // Basic cleanup attempt when the component is destroyed.
    return () => {
      if (widgetContainer && widgetContainer.contains(script)) {
         // widgetContainer.removeChild(script); // Might not work
         // widgetContainer.innerHTML = ''; // Force clear
      }
    };
  });
</script>

<!-- TradingView Widget BEGIN -->
<!-- Use Svelte styling for height/width -->
<div class="tradingview-widget-container">
  <!-- Bind this specific div to the widgetContainer variable -->
  <div class="tradingview-widget-container__widget" bind:this={widgetContainer}></div>
  <div class="tradingview-widget-copyright">
    <a href="https://www.tradingview.com/" rel="noopener nofollow" target="_blank">
      <span class="blue-text">Track all markets on TradingView</span>
    </a>
  </div>
</div>
<!-- TradingView Widget END -->

<style>
  .tradingview-widget-container {
    height: 80vh; /* Default height, adjust as needed or pass via prop */
    width: 100%;
  }
  .tradingview-widget-container__widget {
     /* The 'autosize: true' config should handle this, but we can add fallback */
     height: calc(100% - 32px) !important; /* Match the copyright height */
     width: 100% !important;
  }
  .blue-text {
      color: #2962FF; /* TradingView link color */
  }
 .tradingview-widget-copyright {
    height: 32px; /* Explicit height */
    font-size: 13px !important;
    line-height: 32px !important;
    text-align: center !important;
    vertical-align: middle !important;
    font-family: -apple-system, BlinkMacSystemFont, 'Trebuchet MS', Roboto, Ubuntu, sans-serif !important;
    color: #B2B5BE !important; /* TradingView copyright text color */
 }
</style> 