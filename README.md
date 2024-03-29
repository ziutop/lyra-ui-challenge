# Instructions

This task involves building a simple text UI to quote buying ETH calls and puts on Lyra.

Ask us any questions about the expected final product, and how options or Lyra's SDK works. We prefer thoughtful questions, planning and correctness over completeness.

<a href="https://imgur.com/HIiZFaS"><img src="https://i.imgur.com/HIiZFaS.gif" title="source: imgur.com" /></a>

## Dependencies

1. [React](https://reactjs.org/): A library for building user interfaces
2. [Lyra.js](https://www.npmjs.com/package/@lyrafinance/lyra-js): An SDK to interact with Lyra's smart contracts on our test network
3. [Chakra](https://chakra-ui.com/docs/components/overview): A simple component library
4. [Next.js](https://nextjs.org/): A simple framework for spinning up apps

## Important Notes

1. You have to pay a dollar price for a call or put option, we call this the premium.
2. If the strike price is greater than the spot price (current ETH value), we expect the price to go up and should buy a call. If the strike is lower than spot, we expect the price to go down and should buy a put.
3. Markets have many boards (or expiries), boards have many strikes, and quotes are made on strikes.
4. To quote the premium for a call or put in Lyra's SDK, you need to have a reference to the strike ID, not just strike price. The same strike price across different expiries does not have the same strike ID.
5. Most dollar values in Lyra's SDK are BigNumbers. We provide helper functions to convert between BigNumber and number format.

# Setup

1. Clone and install the application

```
git clone https://github.com/lyra-finance/lyra-frontend-challenge.git
yarn install
```

2. Get an [Infura](https://infura.io/) project ID and set the `NEXT_PUBLIC_INFURA_PROJECT_ID` environment variable.

3. Run the application

```
yarn dev
```

4. Complete the challenge. Edit `src/pages/index.ts` to complete the challenge, and watch `http://localhost:3000` for realtime updates.

# Lyra.js Reference

This line initializes our SDK with a connection to Kovan, Optimistic Ethereum's test network

```
const lyra = new Lyra(Deployment.Kovan)
```

To fetch a market, you can make an async request by market name (e.g. "ETH")

```
const market = lyra.market('ETH')
const spotPrice = market.spotPrice // Current value of ETH
```

To get expiries (boards) for a market, use the boards edge from market

```
const boards = market.boards()
const firstBoard = boards[0] // Earliest board
const expiryTimestamp = firstBoard.expiryTimestamp // Expiry as unix timestamp
```

To get strikes for a board, use the strikes edge from board

```
const strikes = board.strikes()
const firstStrike = strikes[0] // Smallest strike
const strikePrice = firstStrike.strikePrice // Strike price
```

To quote an option premium, use the quote edge from strike

```
const isCall = true
const isBuy = true // Always true for this exercise
const size = toBigNumber(1.0)
const quote = strike.quote(isCall, isBuy, size)
const premium = quote.premium // Option price
```
