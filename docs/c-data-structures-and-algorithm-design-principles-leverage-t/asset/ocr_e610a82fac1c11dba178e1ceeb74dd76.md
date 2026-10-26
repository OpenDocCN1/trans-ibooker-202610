Take a look at the following example:

```
(a1 OR a2) -> TRUE
```

```
(0 V 0) = FALSE
(0 V 1) = TRUE
(1 V 0) = TRUE
(1 V 1) = TRUE
```

```
(a1 AND a2) -> TRUE
```

```
(0 ∧ 0) = FALSE
(0 ∧ 1) = FALSE
(1 ∧ 0) = FALSE
(1 ∧ 1) = TRUE
```

```
(a1 NOT a1) -> FALSE
```

```
(0 ¬ 0) = FALSE
(1 ¬ 1) = FALSE
```

```
(a1 NOT a2) AND (a1 AND a2) -> FALSE
```

```
(0 ¬ 0) ∧ (0 ∧ 0) = FALSE
(0 ¬ 1) ∧ (0 ∧ 1) = FALSE
(1 ¬ 0) ∧ (1 ∧ 0) = FALSE
(1 ¬ 1) ∧ (1 ∧ 1) = FALSE
```

Note:
If you are unfamiliar with logic symbols, ¬ denotes NOT, hence (1 ¬ 1) = FALSE, and (1 ¬ 0) = TRUE. Also, ∧ denotes AND, while ∨ denotes OR.