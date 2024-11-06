java c
COMP3161/9164 24T3 Assignment 2 
Type Inference for Polymorphic MinHS 
Marks :        17.5% of the overall mark 
Due date: Monday 18th November 2024, 12:00 PM Sydney time
Overview In this assignment you will implement type inference for MinHS. The language used in this assignment differs from the language of Assignment  1 in two respects:  it has a polymorphic type system, and it has aggregate data structures.
The assignment requires you to:
•  (100% marks) Implement the type inference  algorithm discussed in the lectures for polymorphic MinHS with sum and product data types;
•  (5% bonus) adjust the type inference pass to include various simple syntax extensions from Assign- ment 1;
•  (5% bonus) adjust the type inference pass to allow optional type annotations provided by the user;
Each of these parts is explained in detail below.
The MinHS parser and evaluator are provided for you.  You do not have to change anything in any module other than TyInfer .hs (even for the bonus parts).
Your type inference pass should return the inferred type scheme of the top-level binding, main.
Your assignment will only be tested on correct programs, and will be judged correct if it produces a correct type for main up to α-renaming of type variables, and reordering of quantifiers.
Submission 
Submit your (modified) TyInfer .hs using the CSE give system, by typing the command
give  cs3161  TyInfer  TyInfer .hs
or by using the CSE give web interface.
1 Task 1 
Task 1 is worth 100% of the marks of this assignment. You are to implement type inference for MinHS with aggregate data structures. The following cases must be handled:
• the MinHS language of the first task of assignment 1 (without n-ary functions, or lists);
• product types: the 0-tuple (aka the Unit type) and 2-tuples;
• sum types;
•  polymorphic functions 
These cases are explained in detail below. The abstract syntax defining these syntactic entities is in Syntax .hs. You should not need to modify the abstract syntax definition in any way.Your implementation is to follow the definition of inference rules provided with this assignment.  In particular, you must implement a variables-in-contexts version of Hindley-Milner type inference  [1] by tracking definitions (in addition to declarations) for type variables in the context.Additional material can be found in the lecture notes on polymorphism, and reference materials [2,3]. The variables-in-contexts approach you will need to implement is outlined in this assignment specification, along with inference rules in Figures 1,2 and 3. 
2 Bonus Tasks These tasks are all optional, and are worth a total of an additional 10%.  Marks above 100% are converted to exam marks, at an exchange rate of 1 to 0.15—for example, a mark of 105% yields 0.75 bonus marks for the exam.
2.1 Bonus Task 1: Simple Syntax Extensions 
This bonus task is worth an additional 5%.  In this task, you should implement type inference for multiple bindings in the one let expression, with the same semantics as the extension task for Assignment 1.
You will need to develop the requisite extensions to the type inference algorithm yourself, but the exten- sions are very similar to the existing rules.
2.2 Bonus Task 2: User-provided type signatures This bonus task is worth an additional 5%.  In this task you are to extend the type inference pass to accept programs containing some type information. You need to combine this with t代 写COMP3161/9164 24T3 Assignment 2 Type Inference for Polymorphic MinHSPython
代做程序编程语言he results of your type inference pass to produce the final type for each declaration.  That is, you need to be able to infer correct types for programs like:
main  =  let  f  ::   (Int  ->  Int)
=  recfun  g  x  =  x;
in  f  2;
You must ensure that the type signatures provided are not overly general.  For example, the following pro- gram should be a type error, as the user-provided signature is too general:
main  ::   (forall  ’a .  ’a)  =  3;
You may assume for simplicity that the user-provided types have distinct type variable names for all bound / free type variables. Your solution should, for example, support programs such as:
main  ::  forall  ’a .  ’a  ->  ’a  +  Int  =  recfun  m  y  =
let  g  ::  forall  ’b .  ’b  ->  ’a  +  ’b  =  recfun  f  x  =  Inl  y;
in  g  1
where the type variable  ’a is in scope for the expression bound to main and appears in the user-provided type annotating g. All occurrences of ’a within the bound expression reference the same type variable.
3 Algebraic Data Types This section covers the extensions to the language of the first assignment. In all other respects (except lists) the language remains the same, so you can consult the reference material from the first assignment for further details on the language.
3.1 Product Types 
We only have 2-tuples in MinHS, and the Unit type, which could be viewed as a 0-tuple.

3.2 Sum Types 
Sum types in MinHS follow the presentation in the lectures.

3.3 Polymorphism The extensions to allow polymorphism are relatively minor.  Three new type forms have been introduced: the FlexVar  t form, the RigidVar  t form, and the Forall  t  e form.   FlexVar  t represents a unification variable introduced during type inference.   RigidVar  t represents  a fixed type variable introduced by a forall-quantifier.  Consult Section 4 for more details on the notational conventions used in this specification and how they relate to the Haskell code.  We distinguish between type schemes and other types:
Type inference should return a correctly typed top-level binding for main.  For example, consider the fol- lowing code fragment before and after type inference:
main  =
let  f  =  recfun  g  x  =  x;
in  if  f  True
then  f   (Inl  1)
else  f   (Inr   ());
main   ::   (Int  +  1)  =
let  f  =  recfun  g  x  =  x;
in  if  f  True
then  f   (Inl  1)
else  f   (Inr   ());
4 Notational Conventions 
In this document, we will use a number of conventions and conveniences to streamline the presentation detailed in Table 1. 

Table 1: Notations and Conventions in this Specification vs. Haskell code for Assignment
Type variable names are ranged over by lowercase greek letters.  We distinguish between flexible and rigid type variables by superscripting such names with F and R, respectively.
Declarations and definitions for flexible type variables appear in typing contexts, see Section 5.1 for the full grammar of contexts.Substitution for type variables occurring in types is explained in Section 6. Since there are two kinds of type variables:  flexible ones introduced during type inference, and rigid ones bound by ∀-quantifiers, we have two kinds of substitution operation depending on which type variables we wish to replace.  These are called substFlex and substRigid in the Subst .hs Haskell module, substituting for flexible and rigid type variables, respectively.



         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
