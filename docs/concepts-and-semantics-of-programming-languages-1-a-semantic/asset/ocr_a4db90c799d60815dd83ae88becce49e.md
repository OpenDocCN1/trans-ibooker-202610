constructor. In section 6.1, we examined a number of basic types used in modern programming languages. In addition to these basic types added to the type algebra, all languages include constructs for defining new types. Type definition provides a response to three orthogonal needs. First, users may wish to create new values different to those of the basic types (such as int or bool). Second, it is sometimes necessary to combine several types in order to create a more complex data structure. Finally, it may be helpful to rename an existing type to make code easier to read and maintain. In all cases, the definition of a type involves adding a new name called type constructor to the type algebra.

The introduction of type definitions modifies the existing equality relation between types. The equality of terms of the type algebra, i.e. structural equality, was used implicitly in Chapter 5; two types were considered to be equal if their terms were identical. When we introduce new type names – either to rename existing types or to create structured types – there are two possible meanings of equality of types. In the case of nominal typing, we consider that two types are equal if and only if they have the same name. Otherwise, the equality is defined by verifying that the structures of the types bound to the names are equal: this is known as structural typing. The choice of one of these options is a semantical decision, and it does not depend on the forms of definition available in a language.

# 6.4.1. Type abbreviations

Type abbreviation consists of naming a type expression. In C and C++, this naming operation is carried out using the typedef construct. In OCaml, a new name is introduced by means of a type definition, which must relate to a simple type expression (i.e. one comprising only type constructors).

Leaving aside the confinement mechanisms presented in Volume 2, Chapter 1. the creation of abbreviations is a means of shortening type expressions. Consider example 6.20, written in C. Instead of const unsigned long long int, we may wish to use a more concise alias, introducing the constructor cullint_t and using it in declarations.

+   EXAMPLE 6.20.–

```
C
typedef const unsigned long long int cullint_t ;

cullint_t t[10] ;

cullint_t add (cullint_t x, cullint_t x) {
return (x + y) ;
}
```