# Table 8.1 Modal identification ensemble statistics.

|  | Modal frequency estimates (relative error(ε)) |  |  |  |  |
|---|---|---|---|---|---|
| Frequency (Hz) | 12 ± σ(%ε) | 13 ± σ(%ε) | 14 ± σ(%ε) | 15 ± σ(%ε) | 16 ± σ(%ε) |
| 190 | 189±0.2(0.5) | 189±0.2(0.5) | 189±0.1(0.5) | 188±0.2(1.1) | 188±0.1(1.1) |
| 208 | 207±0.4(0.5) | 206±0.5(1.0) | 205±0.4(1.4) | 204±0.5(1.9) | 203±0.4(2.4) |
| 219 | 229±2.3(4.6) | 223±0.7(0.5) | 221±1.2(0.9) | 216±0.8(1.4) | 213±0.5(2.7) |
| 242 | 238±0.8(1.7) | 236±0.8(2.1) | 241±1.7(0.4) | 247±1.0(2.1) | 243±0.7(0.4) |
| 260 | 260±2.1(0.0) | 256±0.8(0.4) | 258±3.5(0.8) | 254±0.9(2.3) | 259±1.1(0.4) |
| 276 | — | 276±2.6(0.0) | — | 268±3.1(2.9) | 276±2.1(0.0) |
| 279 | 281±6.3(0.7) | — | 279±5.1(0.0) | 284±3.8(1.8) | — |
| 344 | 347±0.7(0.9) | 344±10.8(0.0) | 346±7.6(0.6) | 348±1.7(1.2) | 349±1.4(1.5) |
| 351 | 353±0.6(0.6) | 350±0.6(0.3) | 352±0.6(0.6) | 353±0.6(0.6) | 354±0.8(0.3) |
| 359 | — | 354±1.4(1.4) | 359±1.9(0.0) | 359±1.9(0.0) | 359±1.1(0.0) |
| 362 | — | — | — | — | 362±0.9(0.0) |
| 364 | 371±0.3(0.0) | 372±0.2(2.4) | 372±0.4(2.2) | 373±0.1(2.5) | 373±0.1(2.5) |
| 377 | 376±0.3(0.3) | 375±0.3(0.5) | 377±9.2(0.0) | 376±0.3(0.3) | 378±0.4(0.3) |
| 383 | — | — | — | — | — |
| Average σ(%ε) | ±1.4 (0.98%) | ±1.8 (0.83%) | ±3.00 (0.67%) | ±1.24 (1.55%) | ±0.8 (0.97%) |

3.6–7.1 s/identification, which is based on the required number of samples for the identification algorithm (ID time × sampling interval). This time is quite reasonable for a 900 Hz sampling frequency and vibrational monitoring of the system. Note that the higher the order, the number of samples for subspace identification increases.

Postprocessing of the tracking frequencies were shown in Figure 8.3 for the 13-mode comparison and Figure 8.4 for the 12 —14-mode tracks. It is clear from the figures that by postprocessing (off-line), the modal frequency tracks are improved from those provided on-line primarily because of the availability of the “batch” statistics. That is, the batch postprocessor performing both outlier correction and tracker smoothing is superior to the on-line tracks primarily because the ensemble statistics of 83 data windows are now available to improve the estimates.

The ensemble statistics for the 13-mode identification are shown in Figure 8.3, where we see the scatter plot along with the corresponding 99.9% confidence interval about the mean modal frequency. Clearly, the estimates are reasonably precise as shown in Figure 8.3a and in Table 8.1 as well. In