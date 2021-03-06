# react-peer

[![Build Status](https://travis-ci.org/madou/react-peer.svg?branch=master)](https://travis-ci.org/madou/react-peer)
[![npm](https://img.shields.io/npm/v/react-peer.svg)](https://www.npmjs.com/package/react-peer)
[![npm bundle size (minified + gzip)](https://img.shields.io/bundlephobia/minzip/react-peer.svg)](https://bundlephobia.com/result?p=react-peer)
[![Dev Dependencies](https://david-dm.org/madou/react-peer/status.svg)](https://david-dm.org/madou/react-peer)
[![Dev Dependencies](https://david-dm.org/madou/react-peer/peer-status.svg)](https://david-dm.org/madou/react-peer?type=peer)
[![Dev Dependencies](https://david-dm.org/madou/react-peer/dev-status.svg)](https://david-dm.org/madou/react-peer?type=dev)

Using the power of [WebRTC](https://webrtc.org/faq/#what-is-webrtc) and [peerjs](https://peerjs.com/) you can send data to someone else's browser as easy as using setState() ⚛🍐

## Installation

Uses [peerjs](https://peerjs.com/) under the hood.
If you want to use the peer hooks make sure to use a version that supports it - at the moment this is `16.8.0-alpha.1`,
else using the latest version of react will be great!

```bash
npm install react-peer react react-dom --save
```

```bash
yarn add react-peer react react-dom
```

## Usage

### `usePeerState<TState>(initialState?: TState, opts?: { brokerId?: string }): [TState, Function, string, Peer.DataConnection[], PeerError | undefined];`

Behaves as your regular `useState` hook,
but will **eventually** send data to any connected peers.
Peers can connect to you using the `brokerId` that is **eventually** returned.

`opts.brokerId` is optionally used when you already have a broker id generated.

```js
import { usePeerState } from 'react-peer';

const App = () => {
  const [state, setState, brokerId, connections, error] = usePeerState();

  setState({ message: 'hello' });
};
```

### `useReceivePeerState<TState>(peerBrokerId: string, opts?: { brokerId?: string }): [TState | undefined, boolean, PeerError | undefined];`

Will receive peer state **eventually** from a peer identified using `peerBrokerId`.

`opts.brokerId` is optionally used when you already have a broker id generated.

```js
import { useReceivePeerState } from 'react-peer';

const App = () => {
  const [state, isConnected, error] = useReceivePeerState('swjg3ls4bq000000');
};
```

### `<PeerStateProvider />`

Useful if not yet using react hooks.
When setting the value prop it will propagate it to all connected peers.

`brokerId` prop is optionally used when you already have a broker id generated.

```js
import { PeerStateProvider } from 'react-peer';

<PeerStateProvider value={{ message: 'hello' }}>
  {({ brokerId, connections, error }) => <div />}
</PeerStateProvider>;
```

### `<ReceivePeerState />`

Useful if not yet using react hooks.
Will receive data from the peer broker.

`brokerId` prop is optionally used when you already have a broker id generated.

```js
import { ReceivePeerState } from 'react-peer';

<ReceivePeerState peerBrokerId="swjg3ls4bq000000">
  {({ data, isConnected, error }) => <div />}
</ReceivePeerState>;
```
