# LankaRemit

**Of every $500 you send to Sri Lanka, how much arrives?**

LankaRemit compares what a bank wire, a money transfer app and USDC on Arc leave for a family in Sri Lanka, then lets you send a real test payment on **Arc Testnet** and see the result for yourself.

Live demo: **https://lankaremit-arc-lk.netlify.app**

Built by [add your name], a student in Colombo, for the Arc community.

## Why

Many Sri Lankan families rely on money sent by relatives working abroad, and a large share of it goes to fees and exchange-rate margins. Arc settles USDC payments with USDC as the gas token, so the cost of the transfer itself is a fraction of a cent. This project makes that difference visible, in English and Sinhala.

## Features

- **Fee comparison** for any amount: bank wire, transfer app, USDC on Arc, shown as bars of what arrives vs. what is kept.
- **Yearly savings** estimate for a monthly transfer, with a household-bill equivalent.
- **Real transaction on Arc Testnet:** connect MetaMask, send test USDC, and see the confirmation time, network fee, block number and explorer link.
- **Wallet balance** (Arc Testnet USDC), shown once connected.
- **Verify a payment:** paste a transaction hash and read the sender, recipient, amount, fee and status straight from the Arc Testnet RPC.
- **Share card:** download a PNG of your result, or copy a text summary.
- **Local history** of your test sends (stored only in your browser).
- **English / Sinhala** interface.

## How it works

It is a single `index.html` file with no build step and no backend.

- Network: Arc Testnet (chain ID `5042002`), RPC `https://rpc.testnet.arc.network`.
- Payment: an ERC-20 `transfer(address,uint256)` call on the USDC contract `0x3600000000000000000000000000000000000000` (6 decimals).
- Confirmation time: the page polls `eth_getTransactionReceipt` and measures the time from broadcast to receipt **in your browser**. This includes your network latency, so it is higher than the time the network itself takes. For the same transaction, the Arc explorer reported a confirmation within about 0.5 s while the page measured 1.33 s.
- Fee: `gasUsed × effectiveGasPrice` from the receipt, shown in USDC.
- Wallet discovery uses EIP-6963, so MetaMask is picked even when other wallets are installed.
- The site never holds funds and never asks for anything beyond a transfer you approve in your wallet.

## Run it

Wallets do not work on pages opened from disk (`file://`), so serve the folder:

```bash
python -m http.server 8000
# then open http://localhost:8000
```

Or drag the folder onto [Netlify Drop](https://app.netlify.com/drop).

You need MetaMask and some Arc Testnet USDC from the [Circle faucet](https://faucet.circle.com).

## Configuration

Edit the `CONFIG` block at the top of the script in `index.html`:

| Setting | Meaning |
| --- | --- |
| `lkrPerUsd` | Illustrative USD to LKR rate |
| `cashOutPct` | Assumed cost of turning USDC into LKR |
| `billLkr` | Illustrative monthly household electricity bill |
| `routes` | Fixed fee and percentage margin for each route |

## Honest limitations

- **Bank and app fees are illustrative estimates**, not quotes from any provider. Replace them with real figures if you have them.
- The USDC route assumes a 0.5% cost to cash out to LKR. Cash-out options and rules in Sri Lanka are not covered here and are not financial or legal advice.
- This runs on **Testnet only**. Testnet USDC has no monetary value.
- The measured confirmation time is observed from the browser, not a network benchmark.

## Screenshots

_Add screenshots of the comparison, a completed test send, the share card and the Sinhala view._

## License

[add a license, for example MIT]
