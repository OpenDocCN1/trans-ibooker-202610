![](bbox=[0.143, 0.12, 0.691, 0.502])

Figure 8.11  Parameter estimation for scintillator voltage response.

corresponding power spectra in (b). The resulting innovations sequence from the identification is shown in Figure 8.10c with a “good fit” indicated from the zero-mean/whiteness test results of (0.000 16 < 0.122/3.5% out) shown in (d). Therefore, the subspace identifier is able to capture the model quite well, and the Kalman filter can be constructed directly using this “black-box” model.

Another approach is to use a MBID technique with the physics-based model embedded requiring estimates of the parameters (K_ab, τ_a, τ_s) from the measured response (averaged) of our scintillator and photomultiplier system shown in Figure 8.7. We applied a nonlinear optimization technique using the well-known Nelder–Mead algorithm. The results of the fit are shown in Figure 8.11 illustrating a reasonable estimate of the response. The final parameter estimates are quite good extracting the truth parameters as K_ab = 1.98, τ_a = 1.03 × 10⁻⁹ seconds, τ_s = 4.7 × 10⁻⁹ seconds and are used in the simulation model for each of our photomultiplier channels.

# 8.2.6  Kalman Filter Design: Scintillation/Photomultiplier System

Now that we have developed a stochastic representation of the scintillator/photomultiplier system, which is commonplace in practice with the