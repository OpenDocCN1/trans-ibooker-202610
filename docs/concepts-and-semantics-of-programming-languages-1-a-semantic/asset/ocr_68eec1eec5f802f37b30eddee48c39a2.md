| τ := int | Integer |
| | bool | Boolean |
| | τ ref | Reference |

# Table 5.2. Type algebra of Lang3

# 5.6.2. Typing rules

Let us begin by setting out the typing rules for expressions in the language. As in the case of the rules presented in Box 5.1, the type relation Env ⊢ e : τ establishes that, in a typing environment Env, an expression e is of type τ.

+   - (T_int) k ∈ Z / Env ⊢ k : int
+   - (T_bool) k ∈ IB / Env ⊢ k : bool
+   - (T_var) Env(x) = τ / Env ⊢ x : τ
+   - (T_unop) ⊢ op : τ1 → τ2, Env ⊢ e : τ1 / Env ⊢ op e : τ2
+   - (T_binop) ⊢ op : τ1 → τ2 → τ3, Env ⊢ e1 : τ1, Env ⊢ e2 : τ2 / Env ⊢ e1 op e2 : τ3
+   - (T_ref) Env ⊢ e : τ / Env ⊢ ↑ e : τ ref
+   - (T_deref) Env ⊢ e : τ ref / Env ⊢ ↓ e : τ

# Box 5.4. Typing rules for expressions in Lang3

Now, let us consider the typing of variable declarations and blocks. A variable declaration d may be used to extend the typing environment by binding the declared identifier to the type of its defining expression. Thus, the relation Env ⊢ d : Env' expresses the fact that, in an environment Env, typing the declaration d will create, from Env, a new environment noted Env'. We recall that a block is written begin d c end, where d is a (possibly empty) sequence of declarations (see section 4.6.1) and c an instruction. We thus need rules for typing a sequence of declarations. Let d d denote a sequence beginning with a declaration d, followed by the sequence d. The empty sequence is noted ∅.

We have chosen to consider that an instruction c has no type as it has no value. Nevertheless, given a typing environment Env, the typing algorithm must check that the expressions involved in an instruction are correctly typed. This typing relation is noted Env ⊢ c : WT (WT denotes well-typed). Note that in certain languages, such as OCaml or certain extensions of C (such as those available with the GCC compiler),