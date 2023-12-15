# Topologies

| Topology          | Pros                                              | Cons                                                                          |
| ------------------| --------------------------------------------------| ------------------------------------------------------------------------------|
| Peer-to-peer      | Minimal central *compute/bandwidth* costs         | Exposes peers to direct DDoS attacks, blocked by some strict NATs             |
| Broker (no logic) | Minimal central *compute* costs                   | Central target for DDoS attacks, still have to pay for bandwidth              |
| Client-Server     | Simpler logic & anti-cheat (authoritative server) | Central target for DoS attacks (including application layer), hosting costs   |

# References

- [What Are STUN, TURN, and ICE?](https://developer.liveswitch.io/liveswitch-server/guides/what-are-stun-turn-and-ice.html)
  - [STUN](https://en.wikipedia.org/wiki/STUN) ≈ Peer-to-peer with possible NAT punchthrough
  - [TURN](https://en.wikipedia.org/wiki/Traversal_Using_Relays_around_NAT) ≈ Broker
  - [ICE](https://en.wikipedia.org/wiki/Interactive_Connectivity_Establishment) ≈ Negotiate both, also possibly use peers as a bridge
- [Steam Networking](https://partner.steamgames.com/doc/features/multiplayer/networking)
  - STUN/TURN (both p2p and broker based networking options)
  - <https://github.com/ValveSoftware/GameNetworkingSockets/blob/master/README_P2P.md>
- [Epic Developer Resources](https://dev.epicgames.com/docs) > [EOS Game Services](https://dev.epicgames.com/docs/game-services) > [NAT P2P Interface](https://dev.epicgames.com/docs/game-services/p-2-p)
  - [Relay Control](https://dev.epicgames.com/docs/game-services/p-2-p#relay-control): "No Relays" ≈ Peer-to-peer, "Force Relays" ≈ Broker
- <http://www.raknet.com/>
