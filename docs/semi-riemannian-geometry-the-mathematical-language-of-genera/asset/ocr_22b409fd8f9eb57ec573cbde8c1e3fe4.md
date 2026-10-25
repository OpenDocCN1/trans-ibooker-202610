# 12.7 Orientation in ℝ³_ν

In Section 12.2, we defined a regular surface to be a regular surface in ℝ³_ν provided its first fundamental form satisfies certain properties. We then proceeded to demonstrate an equivalent formulation based on the existence of a particular type of unit normal vector field. Aside from an increase in geometric intuition, the latter approach offers computational advantages. For example, as remarked in connection with (12.2.4), it is usually more convenient to compute the index of a regular surface in ℝ³_ν indirectly using its sign. In this section, we explore orientation in the context of regular surfaces in ℝ³_ν. The basic definition is given in terms of atlases, but once again unit normal vector fields play a prominent role. In what follows, we rely heavily on the discussion of orientation of vector spaces given in Section 8.2.

# Theorem 12.7.1. Let (M,𝔤) be a regular surface in ℝ³_ν, let (U,φ) be a chart on M, let ℋ = (H₁, H₂) and G be the corresponding coordinate frame and coordinate unit normal vector field, and let q be a point in U. Then (G_q, H₁|_q, H₂|_q) is a basis for ℝ³_ν that has the standard orientation.

Proof. This follows from Theorem 8.4.10(b) and Theorem 12.2.11.

Let (M,𝔤) be a regular surface in ℝ³_ν, and let (U,φ) and (Ũ,φ̃) be overlapping charts on M. Let ℋ and ℋ̃ be the corresponding coordinate frames, and let G and G̃ be the corresponding coordinate unit normal vector fields. Let W = φ(U) ∩ φ̃(Ũ), and let p be a point in W. Recall from Section 8.2 that the coordinate bases ℋ_{φ⁻¹(p)} and ℋ̃_{φ̃⁻¹(p)} are said to be consistent if det( ([id_{T_p(M)}]_{ℋ̃_{φ̃⁻¹(p)}}^{ℋ_{φ⁻¹(p)}} ) ) > 0.

We say that (U,φ) and (Ũ,φ̃) are consistent if ℋ_{φ⁻¹(p)} and ℋ̃_{φ̃⁻¹(p)} are consistent for all p in W.

# Theorem 12.7.2. With the above setup, (U,φ) and (Ũ,φ̃) are consistent if and only if G_{φ⁻¹(p)} = G̃_{φ̃⁻¹(p)} for all p in V.

Proof. This follows from Theorem 8.4.11(b).

Let (M,𝔤) be a regular surface in ℝ³_ν. An atlas for M is said to be consistent if every pair of overlapping charts in the atlas is consistent. We say that M is orientable if it has a consistent atlas. Suppose M is in fact orientable, and let 𝔄 be a consistent atlas for M. The triple (M,𝔤,𝔄) is called an oriented regular surface in ℝ³_ν. Let p be a point in M, let (U,φ) be a chart in 𝔄 at p, and let ℋ be the corresponding coordinate frame. Let 𝔒(p) = [ℋ_{φ⁻¹(p)}],

where we recall from Section 8.2 that [ℋ_{φ⁻¹(p)}] is the equivalence class of all bases for T_p(M) (not just coordinate bases) that are consistent with ℋ_{φ⁻¹(p)}.