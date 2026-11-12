is obtained by, first, constructing the state in which the block instructions are to be executed:

(Env, Mem)
let z = x; →Def₃ ((z, rₓ) ⊕ Env, Mem)
let w = ↑↓x; →Def₃ ((w, r_w) ⊕ (z, rₓ) ⊕ Env, Mem[r_w := 3])
let x = 5; →Def₃ ( (x, 5) ⊕ (w, r_w) ⊕ (z, rₓ) ⊕ Env, Mem[r_w := 3] )
             <---------- Env₁ ----------> <--- Mem₁ --->

Using rule C₈, we obtain the following execution tree:

(Env, Mem) →* Def₃ (Env₁, Mem₁)   (C₃)   ∇₁   ∇₂
                               1       2
(C₈) ---------------------------------------------------------------
                 <Mem, c> →Env Mem₁[r_x := 5][r_y := r_w]
                                    Mem'

where ∇₁ and ∇₂ are the trees:

∇₁ : (C₁) Env₁(z) = rₓ [[x]]Mem₁Env₁ = (5, Mem₁)
<Mem₁, z := x> →Env₁ Mem₁[r_x := 5]

∇₂ : (C₁) Env₁(y) = r_y [[w]]Mem₁Env₁[r_z:=5] = (r_w, Mem₁[r_x := 5])
<Mem₁[r_x := 5], y := w> →Env₁ Mem₁[r_x := 5] [r_y := r_w]

During execution of block c, the environment Env₁ binds the variable x to the value 5, and the variable z to the reference rₓ. Hence, assignment z := x stores the value 5 in the location denoted by rₓ. Allocation also takes place during execution of the block: the environment Env₁ binds variable w to a reference r_w. At the end of execution of this block, environment Env₁ is no longer accessible and this binding ceases to be available. Nevertheless, the assignment y := w stored the reference r_w into the location denoted by r_y, and the contents of the location denoted by r_w thus remain accessible by evaluating the expression ↓↓y.

| The definitions of state (Env, Mem) and block c (both introduced at the beginning of this example), and the construction of state (Env, Mem') such that <Mem, c> →Env Mem', are obtained as follows: