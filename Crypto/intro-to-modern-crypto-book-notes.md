# Introduction to Modern Cryptography Notes

https://www.u-cursos.cl/usuario/777719ab2ddbbdb16d99df29431d3036/mi_blog/r/1_book-introduction_to_modern_cryptography.pdf

This books purpose is to provide *rigorous* info in an *accessible* way.

## Introduction

### Principles
- Formal definitions
- Formal and precise assumptions
- Riorous security proofs

### Prerequisites (see Appendix A)
- Discrete mathematics
- Algorithms
- Helpful: Computability theory
- Need specific familiarity with
  - Basic probability
  - Big-O notation
  - Modular arithmatic
  - Idea of efficient algorithms running in polynomial time

Errata are maintained here:
http://www.cs.umd.edu/~jkatz/imc.html

## Chapter 1

### Basic correctness requirement
- Dec_k(Enc_k(m)) = m

### Kerckhoffs principle
- The encryption scheme should *not* be kept secret. Only the key (and plaintext message) should be kept secret.
- Why?
  - Keeping a short key secret, and sharing it, is easier than keeping a large program secret and sharing it.
  - Algorithm details could be leaked.

### Modern crypto *requires* open design because
- Scrutiny is increased, ensuring greater strength.
- Flaws can be revealed by ethical hackers.
- Reverse engineering of code would reveal the secret algorithm.
- Open design allows for standards.

### Attack scenarios

In order of severity:
- Ciphertext-only attack: Derive PT from CT.
- Known-plaintext attack: Derive PT from CT and PT+CT of other messages.
- Chosen-plaintext attack: Derive PT from PT+CT where attacker can encrypt other PT.
- Chosen-ciphertext attack: Derive PT from PT+CT where attacker can decrypt other CT.
[Distinction between the chosen attacks is unclear]

Historical ciphers and lessons learned from them:
- Shift cipher
  - The key space must not be vulnerable to exhaustive search (currently ~2^70).
- 


=============
Bookmark Page:
26
