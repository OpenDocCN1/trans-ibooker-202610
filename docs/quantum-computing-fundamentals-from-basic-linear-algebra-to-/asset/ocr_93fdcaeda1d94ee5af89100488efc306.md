In 2020, researchers at Harvard University proposed a novel method for using photons for qubits. The qubit would be a single atom in their proposed approach, with the atom’s photons as information carriers. In order to use photons in quantum computation, the photons must interact; however, under normal conditions, photons don’t interact with themselves. The Harvard team proposed using a mirror to reflect photons produced by the atom back to the atom so they would interact with the atom. As of this writing, this is a theoretical proposal and has not yet been realized in a physical device.

# 9.1.2 Electron

Electrons and photons are two of the most obvious ways to implement a qubit. Electron qubits use some property, such as electron spin, to indicate the state of the qubit. For example, an up spin can designate state |0> and a down spin can designate |1>.

There are other variations of using electrons to store qubits. In 1997, David DiVencenzo and Daniel Loss proposed a type of quantum computer that uses the spin of freedom of electrons, which are confined to quantum dots, to physically implement a qubit. This is now called a Loss-DiVincenzo quantum computer. The term _quantum dot_ actually comes from nanotechnology. A device that is only of nanometer scale in one dimension is referred to as a quantum well. A device that is of nanometer scale in two dimensions is called a quantum wire. A device that is of nanometer scale in all three dimensions is known as a quantum dot. The Loss-DiVencenzo uses electrons confined to a three-dimensional nano device to implement qubits.

Other researchers have focused on quantum states of electrons in particular mediums. For example, the Okinawa Institute of Science and Technology has worked on using electrons in liquid helium. Data suggests that electron spin states in liquid helium would maintain coherence longer. Reading the data in the qubit is based on detecting different Rydberg states. Rydberg states are excited states that follow the Rydberg formula. This, of course, requires the understanding of a few additional terms. An _excited state_ is any quantum state that has a higher energy than the absolute minimum (i.e., the ground state). The work of Johannes Rydberg, the Rydberg formula, calculates the wavelengths of a spectral line in a chemical element. This model was later refined by Neils Bohr.

The details of the Rydberg formula are not absolutely necessary for you to understand qubits, but for those interested readers, those details are given. First, the formula depends on the Rydberg constant, symbolized as RH for hydrogen. Then, the Rydberg formula (again using hydrogen as an exemplar) is shown here:

```
\frac{1}{\lambda} = R_H \left( \frac{1}{n_1^2} - \frac{1}{n_2^2} \right)
```

In this formula, the values are as follows:

+   - RH is the Rydberg constant.

+   - λ is the wavelength of electromagnetic radiation in a vacuum.