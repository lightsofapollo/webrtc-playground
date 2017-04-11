# WebRTC playground

This a simple starter project for playing with various WebRTC concepts.

This project includes a standard babel + webpack setup to make things a little
easier (particularly for the UI).

## Getting Started

This project uses yarn (but npm should work too)

```js
yarn
yarn start
```

## Reading

WebRTC is built on top of a number of other technologies:

### Transports

WebRTC uses a unique networking that greatly differs from HTTP 1.1 (but has some similar features as HTTP2).

DTLS: TLS + UDP as a basis for the SCTP networking layer.
SCTP: (via UDP tunneling) Streams + Messages + [Less ahead-of-line blocking](https://en.wikipedia.org/wiki/Head-of-line_blocking)


### Signaling

To establish a connection with a peer we need a way to exchange information.
VoIP applications drove most of the conventions here and use [SIP](https://en.wikipedia.org/wiki/Session_Initiation_Protocol)
to initiate connections.

While WebRTC has a robust signaling system it relies on some external system to
pass data between peers to initialize the connection (i.e. Socket.io).

The process for "signaling" roughly looks like this:

```
caller: CreateOffer - Build the offer (this may contain info like the media streams)
caller: SetLocalDescription (set the offer as the current local description for this session)
caller: ICE - (The caller beings using ICE to find it's own IP)
caller: Send Offer -> callee
caller: Send Ice Candidates -> callee
(somehow the callee gets both the offer and all the ice candidates)
callee: SetRemoteDescription (Register the offer from callee)
callee: Add Ice candidates (sent from caller)
callee: CreateAnswer -> caller
callee: ICE - (The caller begins using ICE to find it's own IP)
callee: Send Answer -> caller
callee: Send Ice Candidates -> caller
(somehow the caller gets messages from the callee)
caller: SetRemoteDescription (Remote description from calle's CreateAnswer)
caller: Add Ice candidates (sent from caller)
```

If everything went well then the caller and callee can now communicate over the
established session (Also see TURN / STUN).

## Further Reading

  - [DTLS](https://en.wikipedia.org/wiki/Datagram_Transport_Layer_Security)
  - [SCTP](https://en.wikipedia.org/wiki/Stream_Control_Transmission_Protocol)
  - [WebRTC MDN](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API)
  - [ICE](https://en.wikipedia.org/wiki/Interactive_Connectivity_Establishment)
  - [TURN](https://en.wikipedia.org/wiki/Traversal_Using_Relays_around_NAT)
  - [STUN](https://en.wikipedia.org/wiki/STUN)

# LICENSE

MIT

See LICENSE.MD
