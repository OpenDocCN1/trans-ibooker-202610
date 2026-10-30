# Kingston SATA M.2 in an ORICO Enclosure

I tested a 240GB Kingston SATA SSD (SUV500M8) in an ORICO M.2 Enclosure for SSD (B08R9DMFFT). The Kingston drive is advertised at up to 500MB/s read and 450MB/s write rates.

![](bbox=[0.18, 0.24, 0.82, 0.45])

Here are the results of the test:

|  | Fio 4K – RAND Read | Fio 4K – RAND Write | Fio 4K – SEQ Read | Fio 4K – SEQ Write |
|---|---|---|---|---|
| Kingston SATA | bw=8.7MiB/s | bw=17.9MiB/s | bw=99.9MiB/s | bw=81.7MiB/s |

# Micron M500DC in a MASSCOOL External Enclosure

I attempted to test an 800GB Micron M500DC SATA SSD in a MASSCOOL 2.5" USB 3.0 external enclosure. ESXi did not see the device when I plugged it in.