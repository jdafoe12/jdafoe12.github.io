---
title: "Exploring Quotients, part 1: quotient sets"
description: "Equivalence relations, partitions, and fibers: how quotient sets identify equivalent elements and reveal the distinctions a function forgets."
date: 2026-10-03
topics:
  - Set theory
  - Quotient sets
  - Abstraction
---

> "The historical development of mathematics (especially in the past couple of centuries) exhibits a consistent, undeniable pattern: first come the problems, whose sources are many and varied, often inspired by the real world. Eventually, _connections_ are made between diverse problems, usually due to common elements that appear in various proofs. Abstract structures are then devised that can "carry" the kind of information that forms the connection... New questions then arise concerning the behavior of the new abstract structures--classification problems, construction of invariants, structure of sub-objects, et cetera. And the process continued with the discovery of new connections among the abstract structures themselves, generating even more powerful abstractions."
>
> - Paul Lockhart, _A Mathematician’s Lament_ (Bellevue Literary Press, 2009), pp. 131–132.
>

In computer science, we usually aim to construct _computational_ objects with certain properties (e.g. a sorting algorithm, operating system, consensus protocol, etc.). These computational objects are typically implemented as programs or otherwise formally described. This makes it possible to reason mathematically about their behavior. In particular, work by Floyd, Hoare, and others developed formal methods for expressing and proving properties of programs[^floyd1967][^hoare1969]. Within such frameworks, we can attempt to prove whether a program actually satisfies some desired properties.

Because computational constructions can be studied as mathematical objects, designing programs can itself be considered a mathematical activity. As Lockhart observes, mathematics advances by making connections between problems via the _ideas contained in their proofs_, which yield new objects with interesting properties. Therefore, the activity of creating new _computational_ objects benefits from studying not just the mathematics behind a single useful construction, but the broader body of mathematical structures and proof methods.

Among these structures are quotient sets, which have connections to many areas of mathematics. When constructing a quotient set, we use an "equivalence relation" over a set to define common properties among the elements, ignoring other distinctions. Interestingly, this notion corresponds to [abstraction](https://www.wikiwand.com/en/Abstraction) in the ordinary sense, where we identify essential commonalities, and ignore irrelevant distinctions.

Often, the _underlying set_ being quotiented has additional _structure_ (for example, its elements may relate in specific ways via operations, etc.). In this case, an important question is whether our identification of elements is compatible with that structure. This compatibility matters because it determines whether the original operations can still be performed after abstraction. When they can, the resulting quotient preserves the relevant structure of the original set while ignoring distinctions that we no longer care about. Regardless, constructing a quotient set creates a world where the introduced notion of equality holds, and we can reason about the consequences of that notion of equality.

Establishing the notion of equivalence used in these constructions depends on a concept in set theory called _binary relations_:

> **Definition.** Let $A,B$ be sets. A set $R$ is a **binary relation** over $A$ and $B$ if and only if $R$ is a subset of $A\\times B$.
>
> An element $a\\in A$ is related to $b\\in B$ by $R$ (equivalently, $aRb$) if and only if $(a,b) \\in R$.

A relation is **homogeneous** if and only if $B = A$ in the above definition. In this post, the term _relation_ mostly refers to a homogeneous binary relation, unless the context indicates otherwise.

There are many important properties that relations can have. For example, the "divides" relation is a _partial ordering_ over the positive integers (i.e. it is reflexive, transitive, and antisymmetric).

The kinds of relations we care about in this context are _equivalence relations_:

> **Definition.**
> A relation $\\sim$ is an **equivalence relation** if and only if $\\sim$ is reflexive, symmetric, and transitive.

> **Definition.**
> A relation $R$ on $A$ is **reflexive** if and only if for all $a \\in A$, $aRa$.

> **Definition.** A relation $R$ on $A$ is **symmetric** if and only if for all $a,b \\in A$, $aRb \\implies bRa$.

> **Definition.** A relation $R$ on $A$ is **transitive** if and only if for all $a,b,c \\in A$, $aRb$ and $bRc$ implies $aRc$.

This corresponds to our typical notion of equivalence with respect to some "equivalence criteria". For furniture, define $x \\sim y$ if $x$ and $y$ have the same number of legs. This relation is clearly reflexive ($x$ has the same number of legs as itself), symmetric (if $x$ matches $y$, $y$ matches $x$), and transitive (if $x$ matches $y$ and $y$ matches $z$, $x$ matches $z$).

Given an element $a \\in A$ and an equivalence relation $\\sim$ on $A$, we can define a new set called the equivalence class $\[a\]\_{\\sim}$ of $a$ to be $\\{b \\in A \\mid a\\sim b\\}$.

> **Lemma 1.1:** Let $a,b \\in A$, $\[a\]\_{\\sim} = \[b\]\_{\\sim}$ if and only if $a\\sim b$.

*Proof.* Let $a,b\\in A$. Suppose that $\[a\]\_{\\sim}=\[b\]\_{\\sim}$. By reflexivity, $b\\in\[b\]\_{\\sim}=\[a\]\_{\\sim}$, so $a\\sim b$ by the definition of equivalence classes. Now, for the other direction, suppose that $a \\sim b$. Since, $\\sim$ is symmetric, we have $b \\sim a$. Now, suppose we have some $c \\in \[a\]\_{\\sim}$. Then, $a \\sim c$ and (by symmetry) $c \\sim a$. Since $\\sim$ is transitive, we have $c \\sim b$, and by symmetry $b \\sim c$. Therefore, $c \\in \[b\]\_{\\sim}$, which implies that $\[a\]\_{\\sim} \\subseteq \[b\]\_{\\sim}.$ The argument that $\[b\]\_{\\sim} \\subseteq \[a\]\_{\\sim}$ is similar, so that we have $\[a\]\_{\\sim} = \[b\]\_{\\sim}$. $\\blacksquare$

This implies that for any element, knowing only a single relation can be sufficient to identify a set where all elements are equivalent with respect to the criteria. Then, it follows that all elements in that class are pairwise related. Indeed, we realize that an equivalence class is determined by the relations of any one of its members. Thus, we call $a$ in the notation $\[a\]\_\\sim$ a _representative_ of the equivalence class $\[a\]\_\\sim$. An equivalence class may be represented by any of its members.

Next, we will show that any equivalence relation determines a set partition, and vice versa. Additionally, every function partitions its domain, and hence induces an equivalence relation. However, many functions induce the _same_ partition, differing only in how they label the pieces. We end our development by showing that every function determines a canonical map to the partition. The result is that every equivalence relation corresponds to a unique partition and a unique canonical map.

> **Definition.** Let $A$ be a set, and let $\\sim$ be an equivalence relation on $A$. The **quotient set** of $A$ by $\\sim$, denoted $A/{\\sim}$, is the set of equivalence classes of elements of $A$:
> $$
> A/{\\sim}=\\{\[a\]\_{\\sim} \\mid a\\in A\\}.
> $$

> **Definition.** Let $A$ be a set. $B \\subseteq \\mathcal P(A)$ is a **partition** of $A$ if and only if:
>
> 1. $\\bigcup B = A$,
>
> 2. for all $C \\in B$, $C \\neq \\emptyset$,
>
> 3. for all $C,D \\in B$, if $C \\neq D$, then $C \\cap D = \\emptyset$.
>

> **Theorem 1.2:** Let $A$ be a set, and let $\\sim$ be an equivalence relation on $A$. Then the quotient set $A/{\\sim} = \\{\[a\]\_{\\sim} \\mid a \\in A\\}$ is a partition of $A$.

*Proof.* For part 1, suppose we have an arbitrary element $a$ of $A$. Since $\\sim$ is reflexive, we have $a \\sim a$, so that $a \\in \[a\]\_{\\sim} \\in A/{\\sim}$. Thus, $A \\subseteq \\bigcup A/{\\sim}.$ Now, suppose we have some element $x \\in \\bigcup A/{\\sim}.$ Then there exists some $S \\in A/{\\sim}$ such that $x \\in S$. Since $S \\in A/{\\sim}$, we have $S = \[a\]\_{\\sim}$ for some $a \\in A$. Thus, $x \\in \[a\]\_{\\sim}$, so $x \\in A$. Hence, $\\bigcup A/{\\sim} \\subseteq A$.

For part 2, let $\[c\]\_{\\sim} \\in A/{\\sim}$. Since $\\sim$ is reflexive, $c \\sim c$, so that $c \\in \[c\]\_{\\sim}$. Thus, $\[c\]\_{\\sim} \\neq \\emptyset$.

For part 3, let $\[a\]\_{\\sim} \\in A/{\\sim}$ and $\[b\]\_{\\sim} \\in A/{\\sim}$, such that $\[a\]\_{\\sim} \\neq \[b\]\_{\\sim}$. By Lemma 1.1, $a \\not \\sim b$. Now, suppose that there is some $c \\in A$ such that $c \\in \[a\]\_{\\sim}$ and $c \\in \[b\]\_{\\sim}$. By the definition of equivalence classes, $a\\sim c$ and $b\\sim c$. Since $\\sim$ is symmetric, we also have $c\\sim b$. But, since $\\sim$ is transitive, we now have $a \\sim b$, which contradicts $a \\not \\sim b$. It follows that $\[a\]\_{\\sim}$ and $\[b\]\_{\\sim}$ must be disjoint. $\\blacksquare$

Now we have the main result mentioned in the introduction: The quotient set takes a notion of equivalence and constructs a set where the equivalent elements are identified while ignoring other distinctions. As an example, let's consider $\\mathbb{Z}/\\equiv\_n,$ where $\\equiv\_n$ is the relation _congruence modulo $n$,_ and $n =3$. In this case, the notion of equivalence is having the same remainder after division by $3$. The only possible remainders are $0,1,$ and $2$, so that $\\mathbb{Z}/\\equiv\_3 = \\{\[0\]\_{\\equiv\_3},\[1\]\_{\\equiv\_3},\[2\]\_{\\equiv\_3}\\}.$ This set is typically referred to as the integers modulo $3$.

In addition, this theorem establishes that any equivalence relation on $A$ can determines a partition of $A$. To show the other direction, that a partition of $A$ determines an equivalence relation on $A$, is left as an exercise.

> **Definition.** Let $A$ and $B$ be sets. A **function** from $A$ to $B$ is a binary relation $f \\subseteq A \\times B$ such that for every $a \\in A$, there exists a unique $b \\in B$ such that $(a,b) \\in f$. Then we write $f: A \\rightarrow B$, and instead of $afb$ or $(a,b) \\in f,$ we write $f(a) = b.$

> **Definition.** Let $f: A\\to B$ be a function. For each subset $S\\subseteq B$, the **preimage** or **inverse image** of $S$ under $f$ is
> $$
> f^{-1}(S)=\\{a\\in A\\mid f(a)\\in S\\}.
> $$

> **Definition.** Let $f: A\\to B$ be a function. For each $b\\in B$, the **fiber of $f$ over $b$** is the preimage of the singleton $\\{b\\}$:
> $$
> f^{-1}(\\{b\\})=\\{a\\in A\\mid f(a)=b\\}.
> $$
> The **fibers** of $f$ are the sets $f^{-1}(\\{b\\})$ for $b \\in B$.

> **Theorem 1.3: Fibers determine an equivalence relation**
> Let $f: A \\rightarrow B$ be a function. Define a relation $\\sim\_f$ on $A$ by
> $$
> a \\sim\_f a^{\\prime} \\iff f(a) = f(a^{\\prime}).
> $$
> Then $\\sim\_f$ is an equivalence relation on $A$. Moreover, the equivalence classes of $\\sim\_f$ are exactly the nonempty fibers of $f$.

*Proof.* That $\\sim\_f$ is an equivalence relation follows from the fact that equality on $B$ is an equivalence relation.

Let $F = \\{f^{-1}(\\{b\\}) \\mid b \\in B \\text{ and } f^{-1}(\\{b\\}) \\neq \\emptyset \\}$. We want to show that $F \\subseteq A/\\sim\_f$.

Let $S \\in F$. Then, $S = f^{-1}(\\{c\\})$ for some $c \\in B$. Since $S$ is nonempty, choose $x \\in S$. By the definition of $f^{-1}(\\cdot)$, $f(x)=c$.

Now let $y \\in S$. By the definition of $f^{-1}(\\cdot)$, $f(y)=c$. Thus, $f(x)=f(y)$, so $x \\sim\_f y$. Therefore, $y \\in \[x\]\_{\\sim\_f}$. Since $y \\in S$ was arbitrary, $S \\subseteq \[x\]\_{\\sim\_f}$.

Now, let $y \\in \[x\]\_{\\sim\_f}$. Then we have $x \\sim\_f y$, which implies that $f(x) = f(y) = c$. Further, this implies that $y \\in S$, by the definition of $f^{-1}(\\cdot).$ Therefore $\[x\]\_{\\sim\_f} \\subseteq S$. Therefore, $S = \[x\]\_{\\sim\_f}$, so $S \\in A/\\sim\_f.$ Since $S$ is an arbitrary element of $F$, we have shown that $F \\subseteq A/\\sim\_f.$

Next, we want to show that $A/\\sim\_f \\subseteq F$. Let $S \\in A/\\sim\_f$.Then, by the definition of quotient sets, $S = \[x\]\_{\\sim\_f}$ for some $x \\in A$. Let $c=f(x)$. Since $f: A\\to B$, we have $c\\in B$. Let $y \\in S$. Then, we have $x \\sim\_f y$, which implies that $f(x) = f(y) = c$. Therefore, $y \\in f^{-1}(\\{c\\}).$ Thus, $S \\subseteq f^{-1}(\\{c\\}).$

Now, let $y \\in f^{-1}(\\{c\\})$. By the definition of $f^{-1}(\\{c\\})$, we have $f(y)= c$. Since $c=f(x)$, we have $f(y)=f(x)$, so $x\\sim\_f y$. Therefore, $y \\in \[x\]\_{\\sim\_f}=S$. Since $y\\in f^{-1}(\\{c\\})$ was arbitrary, $f^{-1}(\\{c\\})\\subseteq S$.

Therefore, $S=f^{-1}(\\{c\\})$. Since $x\\in S$, $S$ is nonempty. Hence $S\\in F$. Since $S$ is an arbitrary element of $A/\\sim\_f$, we have shown that $A/\\sim\_f \\subseteq F.$ Since we have shown that both $F \\subseteq A/\\sim\_f$ and $A/\\sim\_f \\subseteq F,$ we have $F = A/\\sim\_f.$ $\\blacksquare$

> **Definition.** Let $A$ be a set, and let $\\sim$ be an equivalence relation on $A$. The **quotient map** is the function $q:A\\rightarrow A/{\\sim}$ defined by $q(x)=\[x\]\_{\\sim}$.

When $\\sim$ is determined by a function (i.e. $\\sim = \\sim\_f$ for some $f$), we have $q(x) = f^{-1}(\\{f(x)\\})$ by Theorem 1.3.

> **Theorem 1.4: Canonical Decomposition**
> Let $f: A\\to B$ be a function, and define $a\\sim\_f a^{\\prime}$ if and only if $f(a)=f(a^{\\prime})$.
> Then there is an injective function
> $$
> \\bar f: A/{\\sim\_f}\\to B
> $$
> defined by
> $$
> \\bar f(\[a\]\_{\\sim\_f})=f(a).
> $$
> In particular, $f$ can be decomposed as
> $$
> A \\xrightarrow{q} A/{\\sim\_f} \\xrightarrow{\\bar f} B
> $$

*Proof.* First, we need to show that $\\bar f$ is a function. That is, for every $\[a\]\_{\\sim\_f} \\in A/{\\sim\_f}$, there must exist a unique $b \\in B$ such that $\\bar f(\[a\]\_{\\sim\_f})=b$. Existence follows because $f(a)\\in B$. To show uniqueness, suppose that we have $a, a^{\\prime} \\in A$ such that $\[a\]\_{\\sim\_f} = \[a^{\\prime}\]\_{\\sim\_f}.$ We have $a \\sim\_f a^{\\prime},$ and by the definition of $\\sim\_f$, this means that $f(a) = f(a^{\\prime}).$ Therefore, $\\bar f(\[a\]\_{\\sim\_f})=f(a)=f(a^{\\prime})=\\bar f(\[a^{\\prime}\]\_{\\sim\_f}).$ Hence, the value of $\\bar f(\[a\]\_{\\sim\_f})$ does not depend on the representative chosen, so $\\bar f$ is a function.

Next, we want to show that $\\bar f$ is injective. Suppose we have $\\bar f(\[x\]\_{\\sim\_f}) = \\bar f(\[y\]\_{\\sim\_f})$ for $\[x\]\_{\\sim\_f},\[y\]\_{\\sim\_f} \\in A/\\sim\_f.$ By the definition of $\\bar f,$ this implies that $f(x) = f(y)$. By the definition of $\\sim\_f$, this implies that $x \\sim\_f y.$ By Lemma 1.1, it follows that $\[x\]\_{\\sim\_f} = \[y\]\_{\\sim\_f}.$ Therefore, $\\bar f$ must be injective.

Finally, let $a \\in A$. Then $(\\bar f \\circ q)(a) = \\bar f (q(a)) = \\bar f (\[a\]\_{\\sim\_f}) = f(a)$ by following the definitions. $\\blacksquare$

Thus, the quotient map partitions $A$ into equivalence classes. In this way, we have established that the notion of equivalence used in constructing quotient sets can be viewed as being identified by a function $f$, a partition of $A$, or an equivalence relation $\\sim$. Since $\\bar f$ is injective where $f$ was not necessarily injective, our final theorem shows that $q$ collapses the distinctions that $f$ "forgets".

In further posts from this series, we will explore how quotient sets have been used to construct new objects in different domains of mathematics. These developments often take the view that the quotient sets are identified by a function $f$, which is partly the reason we have introduced these notions here.

[^floyd1967]: Robert W. Floyd, “Assigning Meanings to Programs,” in *Mathematical Aspects of Computer Science*, *Proceedings of Symposia in Applied Mathematics*, vol. 19, American Mathematical Society, 1967, pp. 19–32.

[^hoare1969]: C. A. R. Hoare, “An Axiomatic Basis for Computer Programming,” *Communications of the ACM* 12, no. 10 (October 1969): 576–580. https://doi.org/10.1145/363235.363259
