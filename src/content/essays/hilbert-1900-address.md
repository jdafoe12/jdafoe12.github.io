---
title: "Hilbert's 1900 Address: Consistency, Completeness, and the Entscheidungsproblem"
description: "How Hilbert's vision for mathematics led through Gödel's incompleteness theorems and the undecidability of the decision problem to the Turing machine—a theoretical precursor of the modern computer."
date: 2024-08-08
topics:
  - Mathematical logic
  - Computability
  - History of mathematics
---

<div class="portrait-grid">
  <figure>
    <img src="/images/essays/hilbert-1900/hilbert.jpg" alt="Portrait of David Hilbert" loading="eager">
    <figcaption>David Hilbert</figcaption>
  </figure>
  <figure>
    <img src="/images/essays/hilbert-1900/godel.jpg" alt="Portrait of Kurt Gödel" loading="eager">
    <figcaption>Kurt Gödel</figcaption>
  </figure>
  <figure>
    <img src="/images/essays/hilbert-1900/turing.jpg" alt="Portrait of Alan Turing" loading="eager">
    <figcaption>Alan Turing</figcaption>
  </figure>
</div>

> “The history of a science is the science itself.” — Goethe

## Introduction

On this day, August 8, 124 years ago, [David Hilbert](https://en.wikipedia.org/wiki/David_Hilbert) gave a now-famous address at the Second International Congress of Mathematicians (ICM) in Paris, entitled *Sur les Problèmes Futurs des Mathématiques*—“On Future Problems in Mathematics.”[^1]

A founding member of the German Mathematical Society, Hilbert became president of the organization in 1900.[^2] Shortly afterward, he was invited to give an address at the upcoming ICM. To help choose a topic, he sent a letter to [Hermann Minkowski](https://en.wikipedia.org/wiki/Hermann_Minkowski) proposing some ideas. Minkowski later responded:

> What would have the greatest impact would be an attempt to give a preview of the future, i.e. a sketch of the problems with which future mathematicians should occupy themselves. In this way you could perhaps make sure that people would talk about your lecture for decades in the future.

And here we are, “talking” about it over a century later.

Engraved on Hilbert's tombstone are the words *wir müssen wissen, wir werden wissen* (“we must know, we shall know”). These words not only encapsulate his life's work, but also reflect ideas presented in this ICM speech:

> It is probably this important fact along with other philosophical reasons that gives rise to the conviction (which every mathematician shares, but which no one has as yet supported by a proof) that every definite mathematical problem must necessarily be susceptible of an exact settlement, either in the form of an actual answer to the question asked, or by the proof of the impossibility of its solution and therewith the necessary failure of all attempts. Take any definite unsolved problem... however unapproachable these problems may seem to us and however helpless we stand before them, we have, nevertheless, the firm conviction that their solution must follow by a finite number of purely logical processes.
>
> Is this axiom of the solvability of every problem a peculiarity characteristic of mathematical thought alone, or is it possibly a general law inherent in the nature of the mind, that all questions which it asks must be answerable? For in other sciences also one meets old problems which have been settled in a manner most satisfactory and most useful to science by the proof of their impossibility.[^1]

Following Minkowski's suggestion, Hilbert proposed 23 “problems with which future mathematicians should occupy themselves.” Ironically, the subsequent effort to solve a few of these problems would directly undermine his philosophical notions, revealing inherent limits within formal systems and challenging the belief in the complete solvability of every mathematical question.

## Consistency and Completeness

In particular, the results published by [Kurt Gödel](https://en.wikipedia.org/wiki/Kurt_G%C3%B6del) in his 1931 paper “On Formally Undecidable Propositions of *Principia Mathematica* and Related Systems,” in which he proved his two incompleteness theorems, are often seen as a response to Hilbert's second problem. This “second problem” was to prove that arithmetic is consistent:

> **Definition.** A theory $T$ is consistent if there is no formula $\varphi$ such that both $\varphi$ and $\neg \varphi$ are elements of the set of consequences of $T$.[^3]

What Gödel achieved was to relate the idea of consistency to another idea in formal logic: completeness.

> **Definition.** A theory $T$ is complete if, for every formula $\varphi$, either $\varphi$ or $\neg \varphi$ is an element of the set of consequences of $T$.

Significantly, the idea of completeness corresponds to Hilbert's proposition that “every definite mathematical problem must necessarily be susceptible of an exact settlement.”

One theorem that Gödel proved in his 1931 paper is this:

> **Gödel's First Incompleteness Theorem.** Any consistent theory $T$ within which a certain amount of elementary arithmetic can be carried out is incomplete.[^4]

Therefore, if Hilbert's second problem is solved affirmatively—if it is proven that the axioms of arithmetic are consistent—then there will always be true statements that cannot be proven, or untrue statements that cannot be disproven.

As a corollary, Gödel showed the following:

> **Gödel's Second Incompleteness Theorem.** For any consistent theory $T$ within which a certain amount of elementary arithmetic can be carried out, the consistency of $T$ cannot be proved in $T$ itself.[^4]

To demonstrate the consistency of $T$—that is, to solve Hilbert's second problem—one must therefore use a stronger system or external framework. Despite this, the consistency of such theories is usually accepted. Indeed, their inconsistency would have catastrophic consequences. By one of the fundamental principles of logic, known as the principle of explosion (*ex contradictione quodlibet*, or “from contradiction, anything”), the inconsistency of a system implies that *any* statement can be proven within that system, regardless of its truth. This is clearly undesirable.

Chapter XII of *Mathematics: The Loss of Certainty* by Morris Kline[^5] contains a more in-depth discussion of the impact of Gödel's incompleteness results:

> The [first] incompleteness theorem of Gödel has given rise to subsidiary problems that are worthy of mention. Since in any branch of mathematics of any appreciable complexity, there are assertions that can be neither proved nor disproved, the question does arise whether one can determine if any particular assertion can be proved or disproved. In the literature this question is known as the *decision problem*. It calls for effective procedures, perhaps such as computing machines employ, which enable one to determine in a finite number of steps the provability (truth or falsity) of a statement or a class of statements.[^5]

How this came to be known as the decision problem is an interesting story, one that begins again with Hilbert's 1900 address to the ICM. His tenth challenge in this address is as follows:

> Given a Diophantine equation with any number of unknown quantities and with rational integral numerical coefficients: *to devise a process according to which it can be determined by a finite number of operations whether the equation is solvable in rational integers.*[^1]

Essentially, Hilbert's tenth problem is an early formulation of a decision problem.

What Hilbert wants is an *algorithm*, a word that—in a world absent of modern-day computing machines—was not used at the time.[^6] Indeed, this very problem motivated the eventual formalizations necessary to bring such machines into being.

## The Entscheidungsproblem (Decision Problem)

In 1928, Hilbert and [Wilhelm Ackermann](https://en.wikipedia.org/wiki/Wilhelm_Ackermann) published *Principles of Mathematical Logic*, within which the general decision problem was first formally defined:

> It is customary to refer to the two equivalent problems of *universal validity* and *satisfiability* by the common name of the *decision problem* of the restricted predicate calculus... we are justified in calling it the main problem of mathematical logic.[^7]

Such an important problem justifies proper definition and understanding. The following quotations from other sections of the book establish the requisite definitions:

> A formula of the predicate calculus is called logically true or, as we also say, *universally valid* only if, independently of the choice of the domain of individuals, the formula always becomes a true sentence for any substitution of definite sentences, of names of individuals belonging to the domain of individuals, and of predicates defined over the domain of individuals, for the sentential variables, the free individual variables, and the predicate variables respectively.[^7]

This can be seen essentially as an extension of the notion of a tautology in propositional logic. A tautology is a formula in propositional logic that is true under all possible truth-value assignments to its propositional variables. As an example, $P \lor \neg P$ is clearly true both when $P$ is assigned as true and when assigned as false.

A universally valid formula is one that is true under all interpretations in the predicate calculus, also known as first-order logic (FOL). Some examples are the formulas $\forall x (x = x)$ and $\forall x (P(x)) \implies P(c)$. As in the definition above, the formulas hold true for any substitution of definite sentences, names of individuals belonging to the domain, and predicates defined over the domain.

In the example $\forall x (x = x)$, $x$ can be any individual in the domain, and the statement asserts that every individual is equal to itself—a fundamental truth regardless of the specific domain or the particular individuals within it.

For the example $\forall x (P(x)) \implies P(c)$, $P(x)$ is a predicate that can be defined over the domain of individuals. The statement asserts that if $P(x)$ holds for all $x$ in the domain, then it must also hold for a specific individual $c$ from the same domain. The truth of $P$ for all individuals implies its truth for any specific individual, which is true under any interpretation of $P$ and any choice of $c$.

> A formula of the pure predicate calculus, i.e. the one containing no constants, is called *satisfiable* in a domain of individuals if the sentential variables can be replaced by the values truth and falsehood, the predicate variables by predicate constants defined in the domain, and the free individual variables by individual constants, in such a way that the formula goes over into a true sentence. If we call a formula satisfiable, then we mean that there exists a domain of individuals in which it is satisfiable.[^7]

Satisfiability is a specialization of universal validity. Rather than being true under *all* substitutions of definite sentences, names of individuals, and predicates, it is true under *some* substitution of these things in *some* domain. An example is the formula $\exists x (P(x))$.

This example is satisfiable because we can assign a domain of individuals and a predicate $P$ under which it is true. Consider the domain to be all living human beings and the predicate $P(x)$ to mean “$x$ has run 10 miles.” In this context, the formula $\exists x (P(x))$ is interpreted as “there exists a living human being $x$ such that $x$ has run 10 miles.” This is satisfiable because such an individual does exist—for example, [Bernard Koech](https://en.wikipedia.org/wiki/Bernard_Koech), the 10-mile road world record holder at the time of writing.

> In the broadest sense, the decision problem can be considered solved if we have a method which permits us to decide for any given formula *in which domains of individuals it is universally valid (or satisfiable) and in which it is not.*[^7]

The “method” in mind was necessarily a “process... [with] a finite number of operations.” In other words, an algorithm.

To complete our understanding of this definition, we should tie the formal definition back to the problem of finding “procedures... which enable one to determine in a finite number of steps the provability (truth or falsity) of a statement or a class of statements.”

First, we show the equivalence between the problems of determining satisfiability and universal validity. This is based on an established relation: $\varphi$ is universally valid if and only if $\neg \varphi$ is not satisfiable. Contrapositively, if $\neg \varphi$ is satisfiable, then $\varphi$ is not universally valid. Therefore, deciding whether $\varphi$ is universally valid is equivalent to deciding whether $\neg \varphi$ is satisfiable.

Thus, we can reduce the Entscheidungsproblem to finding procedures that determine in a finite number of steps the universal validity of a formula. To connect this to the notion of provability, we can use results from Gödel's 1929 dissertation. This result is known as Gödel's completeness theorem, which establishes that a formula $\varphi$ is provable if and only if it is universally valid.

## Concluding Remarks

If the decision problem were solvable, it would restore some validity to Hilbert's philosophy:

> This conviction of the solvability of every mathematical problem is a powerful incentive to the worker. We hear within us the perpetual call: There is the problem. Seek its solution. You can find it by pure reason, for in mathematics there is no *ignorabimus*.[^1]

However, in 1936 [Alan Turing](https://en.wikipedia.org/wiki/Alan_Turing) and [Alonzo Church](https://en.wikipedia.org/wiki/Alonzo_Church) proved—through the Church–Turing thesis and via the invention of Turing machines and the lambda calculus, respectively—that the Entscheidungsproblem is unsolvable.

This result does not diminish the value of Hilbert's vision, but rather enriches it. The impossibility of an algorithm to decide all mathematical truths ensures that there will always be new problems to solve. The existence of undecidable propositions does not undermine the pursuit of mathematical knowledge and indeed provides “a powerful incentive to the worker.” As [John von Neumann](https://en.wikipedia.org/wiki/John_von_Neumann) wrote in 1927:

> The day that undecidability lets up, mathematics in its current sense would cease to exist; into its place would step a perfectly mechanical rule, by means of which anyone could decide, of any given proposition, whether this can be proved or not.[^8]

---

## Further Reading

<div class="references">

- Edward Frenkel and Lex Fridman, [“Mathematician explains Gödel's Incompleteness Theorem”](https://youtu.be/Osh0-J3T2nY?si=aggVarBsjztgxvUT&t=5361), *Lex Fridman Podcast*.
- Morris Kline, *Mathematics: The Loss of Certainty* (New York: Oxford University Press, 1980), Chapter XII.
- Charles Petzold, *The Annotated Turing: A Guided Tour Through Alan Turing's Historic Paper on Computability and the Turing Machine* (Indianapolis: Wiley Publishing, 2008).
- Stanford University, [“Logic 1 — Propositional Logic”](https://www.youtube.com/watch?v=xL0kNw5TudI), Stanford CS221: AI, 2019.
- Stanford University, [“Logic 2 — First-order Logic”](https://www.youtube.com/watch?v=_Iz83hfkFds&t=2619s), Stanford CS221: AI, 2019.
- P. D. Magnus, [*forall x: An Introduction to Formal Logic*](https://forallx.openlogicproject.org/forallxyyc.pdf).

</div>

[^1]: David Hilbert, *Mathematical Problems*, address delivered to the Second International Congress of Mathematicians in Paris, 1900, [PDF](https://www.aemea.org/math/Hilbert_23_Mathematical_Problems_1900.pdf).
[^2]: [“German Mathematical Society,”](https://en.wikipedia.org/wiki/German_Mathematical_Society) Wikipedia.
[^3]: [“Consistency,”](https://en.wikipedia.org/wiki/Consistency) Wikipedia.
[^4]: [“Gödel's incompleteness theorems,”](https://en.wikipedia.org/wiki/G%C3%B6del%27s_incompleteness_theorems) Wikipedia.
[^5]: Morris Kline, *Mathematics: The Loss of Certainty* (New York: Oxford University Press, 1980), 265.
[^6]: Charles Petzold, *The Annotated Turing: A Guided Tour Through Alan Turing's Historic Paper on Computability and the Turing Machine* (Indianapolis: Wiley Publishing, 2008), 41–42.
[^7]: David Hilbert and Wilhelm Ackermann, *Principles of Mathematical Logic*, trans. Lewis M. Hammond, George G. Leckie, and F. Steinhardt (New York: Chelsea Publishing Company, 1950).
[^8]: Stanford Encyclopedia of Philosophy, [“Church–Turing Thesis and the Decision Problem,”](https://plato.stanford.edu/entries/church-turing/decision-problem.html) accessed August 8, 2024.
