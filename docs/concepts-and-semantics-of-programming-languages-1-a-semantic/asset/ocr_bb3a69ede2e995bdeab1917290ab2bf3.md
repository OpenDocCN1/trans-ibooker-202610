instructions are expressions, meaning that they have values and, consequently, types. The nature of these types depends on choices inherent to the language.

(T_let) Env |- e : tau
Env |- let x = e ; (x, tau) + Env
(T_descseq) Env |- empty : Env
(T_descseq) Env |- d : Env1  Env1 |- d : Env2
Env |- d d : Env2

# Box 5.5. Typing rules for declarations in Lang₃

(T_assign) Env(x) = tau ref  Env |- e : tau
Env |- x := e : WT
(T_skip) Env |- skip : WT
(T_seq) Env |- c1 : WT  Env |- c2 : WT
Env |- c1 ; c2 : WT
(T_loop) Env |- e : bool  Env |- c : WT
Env |- while e do c : WT
(T_cond) Env |- e : bool  Env |- c1 : WT  Env |- c2 : WT
Env |- if e then c1 else c2 : WT
(T_block) Env |- d : Env1  Env1 |- c : WT
Env |- begin d c end : WT

# Box 5.6. Typing rules for instructions in Lang₃

EXAMPLE 5.6.– Consider the GCD computation program presented in section 4.1 and defined by:

```
begin let x = 45;let y = 5;
while not(↓ x = ↓ y) do if ↓ x <= ↓ y then y := ↓ y - ↓ x else x := ↓ x - ↓ y
end
```

Its typechecking tree is:

(T_int) Env |- 45 : int
(T_int) Env1 |- 5 : int
(T_let) Env |- let x = 45 ; Env1
(T_let) Env1 |- let y = 5 ; Env2
(T_descseq) Env |- let x = 45;let y = 5 ; Env2
(T_block) Env |- begin let x = 45;let y = 5; while e1 do i1 end : WT