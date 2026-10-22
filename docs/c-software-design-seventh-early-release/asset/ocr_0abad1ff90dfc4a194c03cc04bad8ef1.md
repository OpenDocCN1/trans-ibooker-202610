The performance improvement for the manual virtual dispatch is extraordinary for GCC. On my system I get down to 1.1476 seconds, which is an improvement of 25% in comparison to the based, unoptimized implementation of *Type Erasure*. Clang, on the other hand, does not show any improvement in comparison to the basic, unoptimized implementation. Although this may be a little disappointing, the runtime is of course still remarkable.

Unfortunately the combination of *SBO* and manual virtual dispatch does not lead to an even better performance. While GCC show a small improvement in comparison to the pure *SBO* approach (which might be interesting for environments without dynamic memory), on Clang this combination does not work as well as you might have hoped for.

In summary, there is a lot of potential of optimizing the performance for *Type Erasure* implementations. If you have been sceptical before about *Type Erasure*, this gain in performance should give you a strong incentive to investigate for yourself. While this is amazing and without doubt is pretty exciting, it is important, though, to remember where this is coming from: only due to separating the concerns of virtual behavior and due to encapsulating the behavior into a value type we have gained these optimization opportunities. We would not have been able to achieve this if all we had would be a pointer-to-base.

# GUIDELINE 33: KNOW ABOUT THE OPTIMIZATION POTENTIAL OF *TYPE ERASURE*

+   - Use *Small Buffer Optimization (SBO)* to avoid expensive copy operations for small objects;
+   - Reduce the number of indirections by implementing virtual dispatch manually.