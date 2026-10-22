Figure 2-9. Dependency structure of the swap () overload set.

# Dependency Inversion Principle vs. Single-Responsibility Principle

As we have seen, the DIP is fulfilled by properly assigning ownership and by properly grouping the things that truly belong. From that perspective, it sounds plausible to consider the DIP as just another special case of the SRP (similar to the ISP). However, hopefully you see that the DIP is more than that. As the DIP, in contrast to the SRP, is very much concerned with the architectural point of view, I consider it a vital piece of advice to build proper global dependency structures.

To summarize, in order to build a proper architecture with a proper dependency structure, it is essential to pay attention to the ownership of abstractions. Since abstractions represent requirements on the implementations, they should be part of the high-level in order to steer all dependencies towards the high-level.

# GUIDELINE 9: PAY ATTENTION TO THE OWNERSHIP OF ABSTRACTIONS

+   - Keep in mind that in a proper architecture, low-level implementation details depend on high-level abstractions;
+   - Adhere to the Dependency Inversion Principle (DIP) and assign abstractions to the high-level of an architecture;
+   - Make sure abstractions are owned by the high-level, not by the low-level.

# Guideline 10: Consider Creating an Architectural Document