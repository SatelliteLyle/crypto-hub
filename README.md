# Predictions Predictor v4.0

**Client-side crypto prediction HUD with live Coinbase WebSocket feed and mobile haptic vibration alerts.**

## Overview

Predictions Predictor is a real-time Bitcoin price prediction engine that analyzes market momentum and provides probabilistic bullish/bearish forecasts. Built with pure client-side JavaScript, it connects directly to Coinbase's WebSocket feed for live BTC/USD data.

## Features

- **Live Price Feed**: Real-time BTC/USD ticker from Coinbase WebSocket
- **Momentum Analysis**: Tracks last 20 price movements for trend identification
- **Probability Display**: Visual bars showing bullish vs bearish momentum percentages
- **Mobile Alerts**: Haptic vibration feedback when bullish probability reaches 75%+
- **Minimal Design**: Clean, dark-themed interface optimized for quick decision-making

## Technical Stack

- **HTML5** - Semantic markup
- **CSS3** - Modern styling with CSS variables and backdrop filters
- **JavaScript (ES6)** - WebSocket client implementation
- **Coinbase API** - Public WebSocket feed (no authentication required)

## How It Works

### Core Algorithm: `calculateContractMetrics()`

```javascript
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
```

**Logic:**
- Analyzes the last 20 price ticks from the WebSocket feed
- Counts upward movements vs. flat/downward movements
- Calculates percentage probabilities based on tick direction
- Returns momentum labels and net dollar change since analysis window opened
- Future-ready for strike price and time decay parameters

### Live Usage

Open `index.html` in any modern browser. The application will:
1. Establish WebSocket connection to Coinbase
2. Subscribe to BTC-USD ticker channel
3. Display live price updates
4. Calculate momentum probabilities in real-time
5. Trigger haptic feedback on strong bullish signals (75%+)

## Status Indicators

- **Connecting...** - WebSocket handshake in progress
- **LIVE** (Green) - Connected and receiving price data

## Browser Compatibility

Requires:
- Modern browser with WebSocket support (all major browsers)
- Haptic vibration support for mobile alerts (optional fallback for desktop)

## Version History

- **v4.0** - Official rebrand as "Predictions Predictor", contract metrics integration
- **v3.8** - PROB-ENGINE initial release with basic momentum analysis

## License

Open source. Use for personal trading research only. Not financial advice.

---

**Created by:** SatelliteLyle  
**Repository:** https://github.com/SatelliteLyle/crypto-hub  
**Live Demo:** https://satellitelyle.github.io/crypto-hub/
