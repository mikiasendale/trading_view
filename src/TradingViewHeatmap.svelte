<script>
  import { onMount } from 'svelte';

  let widgetContainer; // Reference to the inner container div

  onMount(() => {
    const script = document.createElement('script');
    script.type = 'text/javascript';
    script.src = 'https://s3.tradingview.com/external-embedding/embed-widget-forex-heat-map.js';
    script.async = true;
    // Convert the configuration object to a JSON string for the script content
    script.textContent = JSON.stringify({
      "width": "100%", // Use 100% for responsiveness
      "height": "600",  // Adjust height as needed
      "currencies": [
        "EUR",
        "USD",
        "JPY",
        "GBP",
        "CHF",
        "AUD",
        "CAD",
        "NZD",
        "CNY"
      ],
      "isTransparent": false,
      "colorTheme": "dark",
      "locale": "en",
      "backgroundColor": "#1D222D" // Match your app's background if desired
    });

    // Append the script to the specific widget container div after the component mounts
    if (widgetContainer) {
      widgetContainer.appendChild(script);
    }

    // Basic cleanup: remove the script when the component is destroyed.
    // Note: TradingView widgets might have more complex cleanup needs.
    return () => {
      if (widgetContainer && widgetContainer.contains(script)) {
        // widgetContainer.removeChild(script); // Direct removal might not always work depending on how the widget modifies the DOM.
        // widgetContainer.innerHTML = ''; // A more forceful cleanup, potentially clearing other elements if the widget structure is complex.
      }
    };
  });
</script>

<!-- TradingView Widget BEGIN -->
<!-- Apply some basic styling for responsiveness -->
<div class="tradingview-widget-container" style="width: 100%; height: 600px; margin: auto;">
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
  /* Optional: Add styles matching TradingView's defaults or your app theme */
  .tradingview-widget-container {
    /* Add margin or other container styles if needed */
     margin: 20px 0;
  }
  .blue-text {
      color: #2962FF; /* TradingView link color */
  }
 .tradingview-widget-copyright {
    font-size: 13px !important;
    line-height: 32px !important;
    text-align: center !important;
    vertical-align: middle !important;
    font-family: -apple-system, BlinkMacSystemFont, 'Trebuchet MS', Roboto, Ubuntu, sans-serif !important;
    color: #B2B5BE !important; /* TradingView copyright text color */
 }
</style> 