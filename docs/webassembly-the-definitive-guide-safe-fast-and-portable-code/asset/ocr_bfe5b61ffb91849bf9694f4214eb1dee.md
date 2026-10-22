network type it is (i.e. ip4 vs ip6) as well as the preferred transport to use to talk to our node. Notice the distinction between TCP and QUIC over UDP. This is an extremely powerful idea that supports resilience, simple interaction models, and extensibility all throughout the technology stack.

The daemon goes out and looks up bootstrap nodes via DNS. It can use Multicast DNS (mDNS) to find other nodes on the same network. There are many ways for it to communicate with the outside world. But, after a few moments, you can find out who it is talking to as follows. I have elided a ton of results, but we see Multiformat network references for the peers including their node identities and the preferred means to talk to them.

```
brian@tweezer ~/s/ipfs> ipfs swarm peers
/ip4/1.170.45.218/tcp/44262/p2p/QmbsXKVDhxFDgZW6zxrGfgPjXopvhNmGFqqqazv1kyZLkv
/ip4/1.254.1.205/tcp/45622/p2p/QmRePjhxRLWJoAXan79JzvxeUwqW5DyeZbHfxi3yIbSke7
/ip4/101.18.52.217/udp/38214/quic/p2p/12D3KooWBjaFGCZ1heSh4HBy6tsj31348hDeGZhBnRRonCa2JtLT
/ip4/101.70.141.179/udp/7962/quic/p2p/12D3KooWAwcdxRJbDXcYh4FxrNkarnT1BGmqYTedc16oGim4WK8m
/ip4/104.131.131.82/udp/4001/quic/p2p/QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtsmvsqQLuvuj
/ip4/104.207.140.198/udp/4001/quic/p2p/12D3KooWFgmp9SgKqGvcE5zEs1 9iGq16gpMqeV5CYMo2vdD8KiJb
/ip4/104.236.47.160/tcp/4001/p2p/QmXhDHnhAr1PAE6pK1GbxN1Ez5zmkJHq vSN1GHSgPiuLWP
/ip4/104.238.220.184/tcp/4001/p2p/QmRi2tR7Uf33VmKhGBUNZvFEuCnFwaL Lf2FsdYNcLCm4gu
/ip4/104.248.69.187/tcp/4001/p2p/12D3KooWHoqCWMkMuDrauyD6wuJUrPoQ fZGPULj99hX94eu69ps5
/ip4/107.173.84.101/tcp/4001/p2p/12D3KooWNKQGwEMJXqta2uV2xBSsVrN1 jZKd78CF4QossGghaLSe
/ip4/107.184.158.170/udp/35299/quic/p2p/12D3KooWQMeiAvzKWGM7V83EN kNqtgJJhdQeQ2xLAihWvvaaXMuC
/ip4/109.153.171.191/tcp/4001/p2p/12D3KooWK2mqKoGUtZeiJpKCJc3XWLw yk2oC9isNPzvW6ftq9GSv
/ip4/109.194.47.83/tcp/4001/p2p/QmQmfPz9Xn4cNE6vfWcfrozeNDCx9BJFC dRMM3Cnmx2226
/ip4/109.206.48.199/tcp/35317/p2p/QmaY9GdxBY2ovzD1HNhcyBhawTda7gu 6QU8rHv6w4pTfqv
/ip4/111.229.166.178/tcp/4001/p2p/12D3KooWAjVr5JL7VgfoNqT8zro2bp9 fMKUamF2WAYE4syRzBR3z
/ip4/111.92.180.99/tcp/53353/p2p/QmYNmBBbzV7AVytNHmVgoHAKd9CvFRWD
```