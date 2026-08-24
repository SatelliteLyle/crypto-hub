// Replace your old meter logic inside index.html with this updated function
function calculateContractMetrics(prices, strikePrice = null, timeRemainingSeconds = null) {
  if (!prices || prices.length < 2) return null;

  const activePrices = prices.slice(-20);
  const currentPrice = activePrices[activePrices.length - 1];
  const startPrice = activePrices[0];
  const totalTicks = activePrices.length - 1;

  let upTicks = 0;
  let nonUpTicks = 0;

  for (let i = 1; i < activePrices.length; i++) {
    if (activePrices[i] > activePrices[i - 1]) {
      upTicks++;
    } else {
      nonUpTicks++;
    }
  }

  const upTickPct = Math.round((upTicks / totalTicks) * 100);
  const nonUpTickPct = 100 - upTickPct;
  const priceChange = Number((currentPrice - startPrice).toFixed(2));

  return {
    currentPrice: currentPrice.toFixed(2),
    upTickPct: `${upTickPct}%`,
    nonUpTickPct: `${nonUpTickPct}%`,
    labelUp: "BUYING MOMENTUM",
    labelNonUp: "SELLING/FLAT MOMENTUM",
    netDollarChange: priceChange
  };
}
# crypto-hub
Client-side crypto prediction HUD with live Coinbase WebSocket feed and mobile haptic vibration alerts.
