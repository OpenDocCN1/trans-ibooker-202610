
This program, written in Scala, defines a general type `Shape` then two types `Point` and `Line` which "extend" the first type using their own fields. The `perimeter` function uses a value of the general type as input, and examines the precise type of this value. The body of each match case may then use the specific fields coming from the actual type of the examined value.

At first glance, `case class` could be perceived as a different syntactic approach to defining sum types; however, the construct is not simply limited to matching and provides several additional mechanisms used in object-oriented programming.

## 6.6. Equality

In section 6.5, we introduced constructs generalizing the conditional, first to deal with more than two cases, then to provide a notion of "equality" broadened by the pattern matching. While the concept of equality used in programming languages appears straightforward, it merits more detailed examination.

In a mathematical context, equality is defined as a reflexive, symmetric and transitive relation between two elements of the same set. The intuitive semantics of the equality of two elements consists of stating that if, in a mathematical expression, the first is replaced by the second (or vice versa), the value of the expression remains unchanged. The notion of equality was formalized in the 17th century by Gottfried Wilhelm Leibniz, who stated that two values $x$ and $y$ are equal if, and only if, for any predicate $P$, $P(x)$ is true if and only if $P(y)$ is true:

$(x = y) \Leftrightarrow \forall P, P(x) \Leftrightarrow P(y)$

The difficulty with this definition lies in the need for universal quantification on predicates, i.e. on functions returning a truth value. Hence, this definition cannot be directly applied in computer science.

It is often said that two "computer" values are considered to be equal if their representations in the memory are identical. In other terms, the sequences of bits encoding these values must be of the same length and contain two-by-two identical bits. Using the usual semantics for adding integers, it is easy to understand that $4 = 3 + 1$; in other terms, the *values* resulting from the evaluation of the expressions 4 and $3 + 1$ are equal, and both will be represented internally, in binary, as $100_b$.

NOTE.-- The syntax used for equality varies between languages. For example, in C, Java, Python and others, the notation == is used. In OCaml, on the other hand, equality is noted = (== has a different equality semantics) like in Basic, Pascal and Ada. Given that assignment is also noted = in many languages, users are strongly advised to study the syntax of any new language in order to avoid misinterpretations.

There are two possible interpretations when testing the equality of two identifiers x₁ and x₂: we may wish to check whether Env(x₁) = Env(x₂), or whether Mem(Env(x₁)) = Mem(Env(x₂)).

### 6.6.1. Physical equality

In languages such as C, the equality operator == tests the identity of values in the compared expressions. These values must be scalar, otherwise the test will be rejected during the compilation. This point has significant implications for comparing data structures. Generally, the encoding used to construct evolved data structures involves pointers, which are scalar values. Equalities between data structures can relate to the addresses of these structures, in the case of physical equality, or to the values of the structures, known as structural equality, a notion presented in section 6.6.2. In C, when the equality == is used in relation to data structures, it denotes physical equality.

The most striking example of this concerns the comparison of strings. As we saw in section 6.3, strings in C are encoded as arrays of characters (terminating with the character '\0') and the arrays are denoted by pointers (see section 7.3). In testing the equality of two variables representing strings, we are, in fact, testing the equality of the pointers representing these strings, and not equality of the data stored at these locations. This is illustrated in example 6.51.

### EXAMPLE 6.51.–

```c
#include <stdio.h>
int main () {
char *s1 = "Foo" ;
char s2[4] = { 'F', 'o', 'o', '\0' } ;
if (s1 == s2) printf ("Equal\n") ;
else printf ("Different\n") ;
return (0) ;
}
```

This program declares the variable s1, containing the string "Foo" and the variable s2 as a character array containing the same letters as the string "Foo" and ending with a '\0'. Thus, s1 and s2 "contain" identical strings. However, at runtime, this program will return Different, as the locations denoted by s1 and s2 differ.

The semantics of equality in C is even more complex when we consider the case of constant string declarations. The compiler operates by creating only one occurrence of each constant string in a program. Thus, different variables initialized with the same constant string will denote the same reference. A modified version of example 6.51 is shown below, in which s1 and s2 are initialized with the same string "Foo".

### EXAMPLE 6.52.–

```c
#include <stdio.h>

int main () {
  char *s1 = "Foo" ;
  char *s2 = "Foo" ;
  if (s1 == s2) printf ("Equal\n") ;
  else printf ("Different\n") ;
  return (0) ;
}
```

At runtime, the program will now return Equal since the compiler initialized s1 and s2 using the location of a single occurrence of the string "Foo", assigned once and for all in the program code.

As we saw in section 6.3, the difference between declaring a string as a pointer or an array adds another subtle level of difficulty. A declaration of an array will necessarily result in a distinct memory allocation, and will thus produce a negative result in an equality test, even if two variables of this type are initialized using the same string.

In the example below, a subtle change has been made to the program in 6.52. Instead of declaring s2 as a char*, we have chosen to declare it as a char[], i.e. as an array.

### EXAMPLE 6.53.–

```c
#include <stdio.h>

int main () {
  char *s1 = "Foo" ;
  char s2[] = "Foo" ;
  if (s1 == s2) printf ("Equal\n") ;
  else printf ("Different\n") ;
  return (0) ;
}
```

At runtime, this program will return Different since the array designated by s2 is necessarily found at a separate location to the string "Foo" used to initialize both cases.

### 6.6.2. Structural equality

Structural equality relates to the contents of addresses. Two values that are physically equal will be structurally equal, but the reverse is not necessarily true.

Structural equality is only possible in languages with native data structures, and the size and form of the information composing the structure must be known at compile-time. This is the case in both OCaml and Python, which allow structural comparison of lists, arrays, etc. In these languages, the compiler chooses the representation of these structures, unlike in C, where encoding is left to the programmer. The internal implementation of equality draws on knowledge of the compiler’s representation choices. Note that these languages also have the capacity to test for physical equality (== in OCaml, is in Python).

In languages such as C or C++, the compiler cannot automatically determine which semantics to use in comparing two arbitrary data structures. There is no means of knowing whether any given pointer represents either the address of a single entity, or that of an array (and thus of multiple values). This task can only be carried out by the developer, who knows the semantics of the structures which she or he has created. Thus, in the case of a linked list structure, the programmer must implement a comparison which follows the links in the list, knowing that each pointer only provides the address of the next element. In a situation involving arrays of linked lists, the programmer must develop an operation to compare consecutive objects, knowing that the pointer for an array designates the beginning of a sequence of elements. This point is illustrated in example 6.54.

### EXAMPLE 6.54.–

```c
#include <stdlib.h>
#include <stdbool.h>

struct list {
int val ; /* Cell value. */
struct list *next ; /* Address of the next cell. */
};

/* Equality check of two lists denoted by the address of their head
cells. */
bool list_equal (struct list *l1, struct list *l2) {
if (l1 == NULL) {
if (l2 == NULL) return (true) ;
else return (false) ;
}
else {
if (l2 == NULL) return (false) ;
else return ((l1->val == l2->val) && list_equal (l1->next, l2->next)) ;
}
}

/* Equality check of two arrays of lists. The sizes of the arrays are used
as arguments, as they are not included in the array structures. */
bool tab_list_equal (int long1, struct list *t1, int long2, struct list *t2) {
int i ;
```
