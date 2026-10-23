# CHAPTER 3   MATHEMATICAL BACKGROUND AND ITS APPLICABILITY

We’ll present each mathematical concept via the equations and mathematical expressions we will use during the implementation of the algorithms, providing examples of the implementations in C++. The implementations will be presented as case studies, counted from 1 to 10 (see Figures 3-1 through 3-12 and Listings 3-1 through 3-13).

# Preliminaries

In this section, we will present the main concepts, giving the most appropriate definitions of *experiment*, *probability distribution*, *event*, *complementary event*, and *mutually exclusiveness*. The definitions are given in such way that you will find the intersection between theory and practice in a very fashionable and easy way to follow. The concepts described in this chapter will help you get a clear understanding and overview of the basic notions of what a cryptographic and cryptanalysis mechanism stands for and how it is projected using probabilities [1].

**Definition 2.1 [1].** An *experiment* can be seen as a procedure that produces one of a mentioned set of outcomes. Each of the outcomes is individual. The ones that are possible are called simple events. The whole set formed out of the possible outcomes is known as a *sample space*.

In the following sections, we will discuss *discrete sample spaces* that have limited possible outcomes. We will write the simple events of a sample space as S labeled as s₁, s₂, …, sₙ.

**Definition 2.2 [1].** The probability distribution K over S is defined by a sequence of numbers, k₁, k₂, …, kₙ ≥ 0, and the sum of those numbers is equal to 1 (k₁ + k₂ + … + kₙ = 1). The number oᵢ can be interpreted as the *probability* of gᵢ. This is the outcome (result) of the processing experiment.

**Definition 2.3 [1].** The *event* E represents a subset of the sample space S. In this situation, the *probability* that event E will occur, noted as P(E), is defined as the sum of the probabilities oᵢ for all the simple events gᵢ which belong to E. If gᵢ ∈ S, P({sᵢ}) is simply denoted as P(sᵢ).

**Definition 2.4 [1].** Let’s consider E as an event. The *complementary event* is defined as being the set of simple events that don’t belong to E, noted as **Ē**.