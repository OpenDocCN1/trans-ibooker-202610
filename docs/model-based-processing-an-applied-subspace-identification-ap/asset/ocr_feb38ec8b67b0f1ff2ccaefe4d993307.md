![](bbox=[0.18, 0.13, 0.75, 0.62])

# Figure 8.19 Family of sequential Bayesian detection ROC curves for various SNRs: 8.4 dB (P_FA, P_DET)=(0.0, 0.999), 7.4 dB (P_FA, P_DET)=(0.0, 0.9832), 6.9 dB (P_FA, P_DET)=(0.0007, 0.916), 6.4 dB (P_FA, P_DET)=(0.001, 0.76), 2.94 dB (P_FA, P_DET)=(0.04, 0.33).

Note that the true parameter value is shown as the line (dashed) and both the maximum a posterior (MAP) and conditional mean (CM) estimates are shown (arrows) on the plots. They appear to track the physical parameters quite well (small RMS errors) for this realization.

We must perform a sequence of at least 100 realizations and calculate ensemble statistics (not shown). Finally, we show a set of four snapshots (or slices) at various interarrival samples (25, 50, 75, 100) throughout the simulation. Note that each is a slice of the 3D PMFs shown for all of the physical parameters. Also note how the estimated particles coalesce (as expected) about the highest probability regions, which are annotated by the arrows in Figure 8.21.

# 8.3.10 Summary

We have developed a sequential Bayesian approach to the fission detection problem based on a theoretical likelihood PDF capturing the underlying physics of the fission process [30]. A sequential detection processor was