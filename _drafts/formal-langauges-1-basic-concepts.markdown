Formal Languages, Part 1: Basic Concepts
===============

Posted: 2015-02-15 20:22:23
-------------------------

Last week, there was a <a href="http://www.catonmat.net/blog/perl-regex-that-matches-prime-numbers/">link</a>&#160;posted on Hacker News that described a regular expression that only matches non-prime numbers (expressed as a string of "1"s, i.e. in so-called "unary" notation):

```perl
/^1?$|^(11+?)\1+$/
```

<a href="https://news.ycombinator.com/item?id=3391547">Commenters</a> noted that this is not a&#160;<em>real</em> regular expression. But what does that mean? Anyone with experience in Perl regular expressions can see that this clearly will work just fine; the linked article does a good job of breaking down how this goes about matching (or not matching) a given string of 1s. You see, the history of Unix has sort of muddied the term "regular expression" to the point that it means something in practice that&#160;is broader than the theory. Originally, the old Unix command line tool <code>grep</code> was designed with classical "regular expressions" in mind. However, as is well-known, classical regular expressions are very limited, and so a separate tool called <code>egrep</code> was written to accept so-called "extended regular expressions", and eventually <code>egrep</code>'s notion of an extended RE is what we now call a "regular expression".

So what is a "classical" regular expression? To explore that question, we need to look into the field called "formal languages". Despite the name, the field has little to do with programming languages or spoken languages, although it is used in theoretical studies of these kinds of language. Formal language study is the study of patterns, in a way. It asks: what is the simplest pattern which can describe&#160;this (usually infinite) set of strings? So to understand what a regular expression is, we need to set down some definitions first that will allow us to talk about formal languages in general. Once we do that, we can talk about the family of so-called "regular languages" that regular expressions encompass, and go beyond that to define more powerful languages.

<h2>Basic concepts of formal languages</h2>

A&#160;<strong>language</strong> is just a set of&#160;<strong>string</strong>s that are constructed from an&#160;<strong>alphabet</strong>. An alphabet is simply a finite set of symbols such as 'a', 'b', 'c', etc. Traditionally, an alphabet is denoted with the symbol &Sigma;. Strings are composed of zero or more symbols from the alphabet - the so-called&#160;<strong>empty string</strong> is traditionally denoted by&#160;&lambda;.

Since strings can contain many consecutive copies of a particular symbol, we can use a shorthand to represent repetition:

a<sup>n</sup> = aaaaaa&hellip; (n times)

For n = 0, we define 

a<sup>0</sup> = &lambda;

When starting with an alphabet &Sigma;, it's useful to talk about the possible strings it can make. &Sigma;<sup>*</sup> is the set of all strings consisting of zero or more symbols from &Sigma;. So say that our alphabet was very simple: it just consisted of {a}. In this case,

&Sigma;<sup>*</sup> = { &lambda;, a, aa, aaa, aaaa, &hellip; }

or, more succintly,

&Sigma;<sup>*</sup> = { a<sup>n</sup> : n &ge; 0 }

&Sigma;<sup>*</sup> always contains &lambda;; if we want to exclude it, we define

&Sigma;<sup>+</sup> = &Sigma;<sup>*</sup> - { &lambda; }

A language, then, is just a subset of &Sigma;<sup>*</sup> - perhaps in a way we can define succinctly, perhaps not. A string that is a member of a language is called a <strong>sentence</strong>. Remember, these are formal, abstract terms, so don't get too hung-up on the real meaning of "language" or "sentence" here.

Finally, we have the concept of "grammars". Again, this isn't like a grammar you're used to, although it's quite close! Sentences in various languages can only be constructed in certain ways; some (like English) are subject-verb-object, some (like Japanese) are subject-object-verb; even others use different combinations. So a basic grammar rule in English is expressed as:

[sentence] &rArr; [subject] [verb] [object]

Subjects and objects can be further defined as agglomerations of nouns and articles and such. In the same way, we define grammars with our languages and alphabets:

G = (V, T, S, P)

G is a <strong>grammar</strong> described as a combination of three sets and one element:

<ul>
  <li>V is defined as a set of <b>variables</b>; as few as one. These are usually denoted with capital letters such as S, A, B, etc.</li>
  <li>T is defined as a set of <b>terminal symbols</b>. This is almost always the alphabet &Sigma; that we are already familiar with</li>
  <li>S is a single member of V, referred to as the <b>start variable</b>; by convention, it's usually just the letter S. This variable is assumed to be, naturally, the start of any derivation using our grammar.</li>
  <li>P is a set of <b>productions</b> similar to our grammar rule above. Productions can contain any number of variables and/or terminal symbols on either side of the arrow, though we specify that the left side contains at least one symbol, while the right side can be empty (i.e. &lambda;).
</ul>

A language, thus, can be defined as all the sentences such that there is some chain of productions in P called a <b>derivation</b> that produces the sentences - we denote this language as L(G), or the language derived from the grammar G.

To wrap up, here's a simple grammar:

G = ({S}, {a, b}, S, P)

where P is given by the two productions:

S &rArr; aSb
S &rArr; &lambda;

We can derive the sentence "aabb" as so:

S &rArr; aSb &rArr; aaSbb &rArr; aabb

By applying the first rule 0 or more times, followed by the second rule, it's pretty easy to see that this grammar defines a fairly simple language:

L(G) = { a<sup>n</sup>b<sup>n</sup> : n &ge; 0 }

So now we can talk about formal languages. Next time, we'll actually get into regular languages, although along the way we will have to detour and talk about "automata". Robots? Well, not really. More like a robot that defines a language. :)
