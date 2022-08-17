# Won(₩)

```javascript
const formatCashWon = n => {
  if (n >= 1e3 && n < 1e4) return `${+(n / 1e3).toFixed(1)}천`;
  if (n >= 1e4 && n < 1e8) return `${+(n / 1e4).toFixed(1)}만`;
  if (n >= 1e8 && n < 1e12) return `${+(n / 1e8).toFixed(1)}억`;
  if (n >= 1e12 && n < 1e16) return `${+(n / 1e12).toFixed(1)}경`;
  return n;
}

console.log(formatCashWon(1241)); // 1.2천
console.log(formatCashWon(177600)); // 17.8만
console.log(formatCashWon(124114177600)); // 1241.1억
```

# Dollar($)

```javascript
const formatCashDollar = n => {
  if (n >= 1e3 && n < 1e6) return `${+(n / 1e3).toFixed(1)}K`;
  if (n >= 1e6 && n < 1e9) return `${+(n / 1e6).toFixed(1)}M`;
  if (n >= 1e9 && n < 1e12) return `${+(n / 1e9).toFixed(1)}B`;
  if (n >= 1e12) return `${+(n / 1e12).toFixed(1)}T`;
  return n;
}

console.log(formatCashDollar(124114177600)); // 124.11B
```
