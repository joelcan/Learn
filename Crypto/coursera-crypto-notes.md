# Use Cases

1. Single-use keys (example: encrypted email)

2. Multi-use keys (example: file system encryption) -- Requires "more machinery" than for single-use. [What is meant by "more machinery"?]


# Coursera crypto course will cover

https://class.coursera.org/crypto-preview/lecture

## Secure Multi-Party Computation

Where we replace trusted 3d party with p2p protocol and special computation algorithm such that ALL participants generate the result while simultaneously none of the participants can cheat.
Anything that can be done with a 3d party can also be done in a decentralized fashion.

## Privately Outsourced Computation

This is where a "trusted" party can perform computation on already-encrypted data and produce an encrypted result and not know anything about what was being computed.
Only the person who encrypted the data can decrypt the resulting computation!

## Zero Knowledge Proof of knowledge

- Given: N = p . q
- Alice can prove to Bob that she knows p and q without revealing p or q.
- Alice can derive a proof R that she knows p and q. [is this part correct?]
- Alice knows N, p, q and R.
- Bob knows N and R only.
[I am not sure, but R may not be a single piece of info. Instead, it may be an ongoing interaction between Alice and Bob.]


# Always follow three rigorous steps in crypto

- Precisely specify the threat model (how to attack and goals)
- Propose a construction
- Prove that breaking construction under threat model will solve an underlying hard problem.


# History of Cryptography

"The Code Breakers"

## Symmetric Ciphers

```
c := E(k,m)
m := D(k,c)
```

## Static Substitution Cipher

All are badly broken because you can crack it analyzing the cipher text alone.
How to encrypt: Replace one letter with another.

How to attack:
1. Calculate the size of the keyspace: 26! ~= 2^88 (~88 bits)
2. Note letter usage frequencies English:
   - E = 12.7%
   - T = 9.1%
   - A = 8.1%
3. All other letters occur with about the same frequency, so look at digram frequencies:
   - HE
   - AN
   - IN
   - TH
4. Look at trigram frequencies etc.

## Vigener cipher

How to encrypt: `c := mod(k+m, 26)`
With the Vigener cipher, the key is shorter than the message.

How to attack:
- Assume we know length of k. The length is L. In reality we set L=1 and perform the following steps, then L=2 and so on until we identify the letter "E" as described below.
- Divide c into groups of length L.
- Look at the 1st letter of every group and identify the most commonly occurring ciphertext letter.
- If the most common letter occurs about 12.7% of the time, then you know that letter must be "E".
- Repeat steps 3 & 4 for the 2nd letter in the group and so on for all
letters in the group.

Next, rotor machines were created.
Same approach as Vigener, but the letters in k rotate as you encrypt each letter.
Can attack rotor machines using frequencies of: letters, digrams, trigrams, etc.

## DES (Data Encryption Standard)

DES is not secure anymore.
- Number of keys (keyspace) = 2^56 (56 bit key)
- block size = 64 bits (encrypts 8 characters at a time instead of just one -- 8 characters x 8 bits per character = 64 bits)

After DES comes AES which uses a 128 bit key.


# Discrete Probability (How to academically prove encryption security)

See also:
https://en.wikibooks.org/wiki/High_School_Mathematics_Extensions/Discrete_Probability

U is a finite set. 
Example: `U = {0,1}^n`  (U stands for "universe")
- `{0,1}^2 = {00, 01, 10, 11}`
- Number of possible items in set is 2^2
- The first 2 is the available alphabet, the second 2 is the message length.

Probability distribution P over universe U is a function:
- `P: U -> [0,1]`
- Each item in the universe is assigned a number between 0 and 1.
- The sum of all these numbers in U must sum to 1.

Example:
- P(00) = 0.5
- P(01) = 0.125
- P(10) = 0.25
- P(11) = 0.125
- Sum of all these items is 1.

## Examples of probability distribution types

1. Uniform distribution: for all x in `U: P(x) = 1/|U|`
   Note that |U| is the size of U (number of items in U).
   Example:
   - P(00) = 0.25
   - P(01) = 0.25
   - P(10) = 0.25
   - P(11) = 0.25
   - Sum of all these items is 1.
2. Point distribution at x0: P(x0)=1; for all x!=x0: P(x)=0.
   Example:
   - P(00) = 0
   - P(01) = 1
   - P(10) = 0
   - P(11) = 0
   - Sum of all these items is 1.

Can write P as a vector of probability values: 
Example: `P:U = P:{0,1}^3 = (P(000), P(001), P(010), ... , P(111))`
This example is a vector of dimension `8 = 2^3 = |U|`

Notes: 
- An inverted A means "for all".
- An inverted U means "intersection".
- A regular U means "union".
- A rounded E means "element of" or "in".
- A rounded C with underline means "subset".
- A backwards E means "there exists".
- A plus inside a circle is XOR. (XOR is just addition modulo 2)
- Pr[x] means "probability of x".
- "lsb" means least significant bit(s).
- "msb" means most significant bit(s).
- "rv" or "r.v." means "random variable"
- "st" or "s.t." means "such that"

## Event

For A, which is a subset of `U: Pr[A] = Sum over x in A of P(x) is in range [0,1]`.
A is called an "event".

## The Union Bound

For events A1 and A2: `Pr[A1 Union A2] <= Pr[A1] + Pr[A2]`

Special Case: 
- If Pr[A1 Intersection A2] = 0, 
- then Pr[A1 Union A2] = Pr[A1] + Pr[A2]

## Random Variables

Defn: random variable X is a function   `X:U -> V`
Ex: `X:{0,1}^n -> {0,1} ;  X(y) = lsb(y)  in {0,1}`
[** I do not get this definition]

## The Uniform Random Variable

A uniform random variable over U is denoted: `r <-R- U`
(The "R" is actually above the backwards arrow.)

For all a in `U: Pr[ r=a ] = 1/|U|`
(formally, r is the identity function: `r(x)=x for all x in U`.)
[** I do not get this definition]

Ex: Let r be a uniform random variable on {0,1}^2
Define the random variable `X=r1+r2`
Then `Pr[X=2] = Pr[r=11] = 0.25`

## Randomized Algorithms

Deterministic algorithm: `y <-- A(m)`
So, for a given input m, A(m) always produces the same output y.

Randomized algorithm: `y <-- A(m;r)  where r <-R- {0,1}^n`
(function A() has an implicit argument r)
So, for a given input m, A(m) most likely produces a different output y.
Output is a random variable:  `y <-R- A(m)`

Ex: `A(m;k) = E(k,m) ,  y <-R- A(m)`

## Independence

[** Look at WikiBooks article on Independence]

Events A and B are independent if  `Pr[A and B] = Pr[A] . Pr[B]`
(dot "." is being used to represent multiplication -- put space before & after.)

For each a,b in `V: Pr[X=a and Y=b] = Pr[X=a] . Pr[Y=b]`
Ex: `U = {0,1}^2 = {00, 01, 10, 11}  and r <-R- U`
Define r.v. (random variables) X and Y as: X=lsb(r) ,  Y=msb(r)

`Pr[X=0 and Y=0] = Pr[r=00] = Pr[X=0] . Pr[Y=0] = 0.5 . 0.5 = 0.25`

[** To understand statistical independence I need an example of the opposite of independence. I want to know how to deductively determine if events are not independent.]

## Why is XOR used so frequently in crypto?

Thm: 
Y is a rand. var. over {0,1}^n, X is an indep. uniform var. on {0,1}^n
Then Z := Y xor X is always a uniform random variable on {0,1}^n

Proof (for n=1):

```
Pr[Y=0] = p0
Pr[Y=1] = p1
Also, p0 + p1 = 1

Pr[X=0] = 0.5
Pr[X=1] = 0.5

Pr[Y=0 and X=0] = Pr[Y=0] . Pr[X=0] = p0/2
Pr[Y=0 and X=1] = p0/2
Pr[Y=1 and X=0] = p1/2
Pr[Y=1 and X=1] = p1/2

Pr[Z=0] = Pr[ (x,y)=(0,0) or (x,y)=(1,1)]
Pr[(x,y)=(0,0)] + Pr[(x,y)=(1,1)]  = p0/2 + p1/2  = (p0 + p1)/2  = 1/2
```

## The Birthday Paradox

Let r1, ..., rn in U  be independent identically distributed random variables.
h
Thm: When `n=1.2 x |U|^1/2`  then `Pr[there exists i,j where i!=j: ri=rj] >= 0.5`
( reminder that x^1/2 is the same as sqrt(x) )
[what is 1.2 for?]

Ex:
Let `U={0,1}^128`
(so, `|U| = 2^128`)
After sampling about 2^64 random messages from U,
about two of the sampled messages will likely be the same.

Ex #2:
Assuming birthays are uniformly distributed through the year (which is false in reality), you would need about 23 people in a group before two of them had the same birthday: `1.2 x 365^1/2 ~= 22.93`

[** I do not understand this reasoning]
[Also look up "collision probability"]


# Stream Ciphers (week 1)


## Information theoretic security and the one time pad
[Not presented yet]

Def: A "cipher" is defined over (K,M,C)
- K = set of all possible keys
- M = set of all possible messages
- C = set of all possible ciphertexts
is a pair of "efficient" algorithms (E,D) where
`E: K x M -> C,  D: K x C -> M`
- E = encryption (is often radomized)
- D = decryption (is always deterministic)

E & D must satisfy the correctness property (or consistency equation):
For every m in M, k in `K: D(k,E(k,m)) = m`

Theoretical definition of "efficient": Runs in polynomial time.
Practical definition of "efficient": Runs in a specified time period for a certain size of input.

### The One Time Pad (OTP)

This is a perfectly secure cipher.
The message space is M.
The cipher space is C.
The key space is K.
The message space is: `M = C = {0,1}^n`
The key space is: `K = {0,1}^n`
The key is a random bit string that is as long as the message.

```
c := E(k,m) = k xor m
m := E(k,c) = k xor c = k xor k xor m = o xor m = m
k := m xor c
```
  This is true because:
  ```
  c = k xor m
  k xor c = k xor k xor m = 0 xor m = m
  k xor c xor c = m xor c
  k xor 0 = m xor c
  k = m xor c
  ```

OTP is very fast encrypting and decrypting, 
but requires very long keys (the key must be at least as long as the message).

### Information Theoretic Security

(aka, what makes a cipher secure)
Ciphertext should reveal no "info" about plaintext.

Def: A cipher (E,D) over (K,M,C) has "perfect secrecy" if
For all m0,m1 in M where (len(m0)=len(m1)) and for all c in C
`Pr[ E(k,m0)=c ] = Pr[ E(k,m1)=c ]`
where k is uniform in the keyspace K.  ( `k <-R- K` )

In other words, given only the ciphertext, I cannot tell if message is m0, m1, ... (for all m values).
This means that ciphertext-only attacks will never work (but note that there are other types of attacks).

Lemma: OTP has perfect secrecy. Where "perfect secrecy" means that success in defeating the security does not improve with greater computational power.

Proof:
For all m,c: `Pr[E(k,m)=c] = (number of keys, k in K s.t. E(k,m)=c) / |K|`
If, for all `m,c: |{k in K s.t. E(k,m)=c}| = const`
Then the cipher has perfect secrecy because:
  ```
  For OTP, if E(k,m)=c
  k xor m = c
  k = m xor c
  |{k in K: E(k,m)=c}| = 1, for all m,c
  ```
  [** why?]

The disadvantage of OTP is that the key must be as large as the message itself and the key must be communicated securely, but if we can communicate the key securely then why not just communicate the message itself because it is the same length?

Shannon also proves that if a cipher has perfect secrecy then it must be true
that: `|K| >= |M|`  (key length must be at least as long as the message)
[I thought that |K| meant the size of the key space, not length of key?]


## Stream ciphers and pseudo random generators
[Presented]

### Stream cipher

With a stream cipher, we convert a short random key into a long "pseudorandom" key using a PRG (pseudo random generator). The short random key is also called a "seed" for the PRG. This longer pseudorandom key is then used in the OTP algorithm. This makes the OTP practical.

Characteristics of a PRG:
- The stream cipher is by definition *not* perfectly secure because the key is shorter than the message. So we will need a new definition of security other than "perfectly secure/secret."
- The pseudo random key space must be "much larger" than the seed key space.
- The PRG must be a deterministic algorithm.
- The PRG must compute "efficiently".  ("Efficient" means in polynomial time or within a specified time period given a specified size of input.)
- The PRG must be "unpredictable".

The PRG is a function:  `G: {0,1}^s -> {0,1}^n`
Where s is a small seed space and n is a large key space. So, n must be much larger than s:  `n >> s`
The PRG is denoted as G(k), where k is the random seed.
So, `c = E(k,m) = m xor G(k)`
`m = D(k,c) = c xor G(k)`

### Define Unpredictable

The PRG must be "unpredictable" according to this definition:
If there exists i st: G(k), bits 1 through i of G(k) can be input into an algorithm A and the output of A can efficiently predict bit i+1 with probability >= 1/2 + e, then G is not unpredictable.
Where e is "non-negligible".

This definition of non-negligible allows us to define security against a specific set of adversaries whose computational power is bounded.

### Define Non-Negligible

In practice:
- e is a scalar.
- Non-negligible: e >= 1/2^30  (likely to happen over 1GB of data)
    About one one-billionth.
- Negligible: e <= 1/2^80  (will never happen over life of key)
    About one one-trillion quintillionth.
- (these definitions are problematic though)

In theory:
- Informally, a function is Negligible if it is "small enough" for all practical purposes. For example, if you have a procedure that can e-distinguish between the output of a PRG and TRG ("true random generator"), then one can boost this procedure by repeating it and get a 0.5 distinguisher. This boosting only works if e is non-negligible, because you are only allowed to repeat the original distinguisher polynomially many times.
- Formally, when a function e is Negligible, we first narrow down our interests to functions that take as input a non-negative integer L (the security parameter) and output a non-negative real number. [Why?]
- e is a function of a security parameter:  `e: Z^(>=0) --> R^(>=0)`
    In plain English: the function e, normally denoted with the character epsilon, acts on non-negative integers (Z means integers) and produces non-negative real numbers (R means real numbers).
- The security parameter L is a parameter that the performance of the system is measured against, for example, a distinguisher should run in "polynomial time". Normally L is denoted by the character lambda.
- Non-negligible: There exists `d >= 0 st e(L) >= 1/L^d infinitely often`
  (e >= 1/polynomial, for many L)
  (or, we say that e has "inverse plynomial growth")
  Equivalently, a function e is non-negligible if `e(L) = Omega(1/L^d)`
  Note: Omega is the "sample space".
- Negligible: For all `d, L >= L0: e(L) <= 1/L^d`
  (e <= 1/polynomial, for large L)
Where:
- L is polynomial time. [But what does that mean?]
- L0 is a real number. [But what is it? And why is it real?]
- Also, there is some d>=0 and real L0 st for all `L >= L0, e(L) >= L^d`. [WTF?]
- d is the degree of the polynomial.
- L^d is the polynomial in question, it is a polynomial in L. [?]
- e is a "distinguisher" [?]
Ex:
- e(L) = 1/2^L  (is negligible because 1/2^L < 1/L^d, always)
- e(L) = 1/L^1000  (is non-negligible because it is possible that 1/L^1000 > 1/L^d)

This next part taken from:
    http://crypto.stackexchange.com/questions/5832/what-exactly-is-a-negligible-and-non-negligible-function

Generally, we assume an adversary is bounded to run in time polynomial to L where L is the security parameter given to G(). More precisely, G() is given input 1^L so that L will be its input size and its output -- the key -- will be polynomial in the size of its input.

[** I do not understand this section]

### Weak PRGs -- do not use these

Linear congruential generator.
- Even though it has good statistical measures, it is weak.
- glibc random() function is related to this type of PRG.
- `r[i] = a . r[i-1] + b mod p`


## Week 1.8 (Lesson 2.3): Attacks on stream ciphers and the one time pad
[Presented]

### Attack 1: Two time pad attack

Never use a stream cipher key more than once.
This is a *very* common mistake.

Alice re-uses key k:
```
c1 = m1 xor PRG(k)
c2 = m2 xor PRG(k)
```

Eve: `c1 xor c2 --> m1 xor m2`  [How to actually decrypt m1 xor m2? Brute force?]

Real World Example: MS-PPTP (on Win NT)
- The client and server used the same key.
- When using a shared symmetric key, it is important to actually use a *pair* of shared keys k1 is for communicating from A to B and k2 is for communicating from B to A.

Real World Example: WEP (wifi 802.11b)
- Prepends a 24 bit counter with k for the key. 
- Problem 1: On some devices this counter is reset to 0 when device is power cycled. 
- Problem 2: After 2^24 frames the counter wraps around and starts at 0 again. An attacker can inject packets repeatedly so as to speed up the counter such that WEP can be cracked in minutes.
- Problem 3: PRG produces the same k in many cases (after about 10^6 frames k can be recovered, and 40k frames is usually sufficient).
- It would have been better to generate one long PRN and divide it into chunks, each chunk would be the key that encrypts each frame.

Some other things I view as issues:
- Possible problem 4: The counter is sent unencrypted with the ciphertext. 
- Possible problem 5: Should not simply prepend the counter to the key, instead should be mixed (such as by hashing).

Real World Example: Disk encryption
- Change one small part of a file and save it encrypted.
- The problem with a stream cipher is that it encrypts as it goes through the data, so it is obvious when one small part changes.

Defense against two time pad attack: Never use stream cipher key more than once.
- For network traffic negotiate new key for every session (e.g. TLS)
- For disk encryption do not use stream ciphers.

### Attack 2: Malleability attack

OTP is malleable meaning that the plaintext can be modified by an attacker and there is no way for the key holder to know that the message was modified.

Alice: `c1 = m1 xor k`

Eve can actively attack the message: `c2 = c1 xor p`

Bob does not know the plaintext has been tampered with:
`m2 = c2 xor k  = c xor p xor k  = m1 xor k xor p xor k  = m1 xor p`

If Eve knows that a message is from "Bob", and if she knows where in the ciphertext the From field is, she could do the following: `p = "Bob" xor "Eve"`


## Week 1.9 (Lesson 2.4): Real-world stream ciphers

### RC4

RC4 is an old system. Do not use it.
Begin with a 128 bit seed and expand to 2048 bits. Release 1 byte per round.
Used in HTTPS and WEP.

Weaknesses:
- Bias in initial output, for example: Pr[ 2nd byte = 0 ] = 2/256 
  - Expected 1/256, if you must use RC4 then ignore the first 256 bytes.
- Prob. of two zero bytes in a row (0,0) is 1/256^3
  - Expected 1/256^2
- Related key attacks -- if two keys are "closely related" then it is possible to recover the keys.

### CSS

CSS, for DVDs, is badly broken. It uses a linear feedback shift register (LFSR).
These algorithms also use LFSRs and are also broken: GSM (A51 & A52), Bluetooth (E0).
[Are *all* LFSR algorithms broken? Or, are there some LFSRs that are strong?]

How an LFSR works:
- An LFSR is where we xor specific slots in the array of bits to produce a bit of output. Then all the bits shift by one and the process repeats.
- For example: Given an 8 bit array, x, suppose the LFSR algoritm is 
- y = x[2] xor x[3] xor x[7], so y is the first bit of output.
- Then we shift all the bits in x to the right by one, remove the rightmost bit, and add a new bit in on the left.
- Then repeat that process to generate the next bit.

CSS Design:
- seed = 5 bytes = 40 bits (export limitation)
- LFSR 1: 17-bit
- LFSR 2: 25-bit
- Initial state of LFSR1 is 1bit||first 2 bytes of key.
- Initial state of LFSR2 is 1bit||final 3 bytes of key.
- Block output = 8 bits of LFSR1 and 8 bits of LFSR2, added and mod 256.

CSS Attack:
- Easy to break in about 2^17 time.
- MPEG is being encrypted, so we know there is a 20 byte prefix before the plaintext.
- Take the first 20 bytes of ciphertext and xor it with the known plaintext. This gives us the first 20 bytes of PRG output.
- Try all 2^17 possible values of LFSR1 for 20 bytes worth (this can be calculated and stored ahead of time).
- For each 20 byte attempt, subtract the 20 bytes of PRG output. This gives us the initial state of LFSR2 which gives us the full key.

### eStream (2008)

eStream is a set of good stream ciphers.

eStream High-Level Design:
- PRG: {0,1}^s x R --> {0,1}^n,  where n>>s
- The seed is {0,1}^s.
- The nonce is R. A nonce is a *non-repeating* value for a given key.
- E(k,m;r) = m xor PRG(k;r)
- A nonce allows you to re-use the key because every key+nonce combination is always unique.

eStream Salsa 20 Overview (for both SW & HW implementations):
- {0,1}^(128 or 256) x {0,1}^64 --> {0,1]^n  (max n = 2^73 bits)
- The seed is {0,1}^(128 or 256).
- The nonce is {0,1}^64.
- Salsa20(k;r) --> H(k,(r,0)) || H(k,(r,1)) || ...

First, construct a 64 byte array a0:
- t0: 4 bytes, pre-defined by algorithm
- k: 16 byte key
- t1: 4 bytes, pre-defined by algorithm
- r:  8 byte nonce
- i:  8 byte incrementing value
- t2: 4 bytes, pre-defined by algorithm
- k: 16 byte key
- t3: 4 bytes, pre-defined by algorithm

How Salsa 20 generates a PRG:
- Apply function h() to the array, which results in a new 64 byte array: a1 = h(a0)
- Repeat h() 10 times by inputting the prior output: a10 = h(a9)
- Then xor the final output with the original (64 byte) input -- this is the first PRN from this algorithm: H = a0 xor a10
- To generate the next PRN, repeat the above, but construct a0 with i incremented by one.


## Week 1.10 (Lesson 2.5): PRG Security Definitions

Let `G: K --> {0,1}^n` be a PRG.

Goal: Define a distribution that is indistinguishable from random.
- So: `[k <-R- K, output G(k)]`
- Must be indistinguishable from: `[r <-R- {0,1}^n, output r]`

### Statistical tests

Statistical test on {0,1}^n:
- An algorithm, A, st A(x) outputs 0 or 1 (0=not rand., 1=rand.)

Examples:
- `A(x)=1` iff  `|number of zeros in x - number of ones| <= 10 . sqrt(n)`
  - [Where did we get 10 . sqrt(n) ?]]
- `A(x)=1` iff  `|number of times we see 00 in x - n/4| <= 10 . sqrt(n)`
- `A(x)=1` iff `maximum run of zeroes in x <= 10 . log2(n)`
- There are hundreds of statistical tests of randomness.

### Advantage

"Advantage" is how to test if a statistical test is good:
- Let `G: K --> {0,1}^n` be a PRG and  A is a stat. test on {0,1}^n.
- Terminology: "AdvPRG[A,G]" means "the advantage of the statistical test of A relative to G."
- Define: `AdvPRG[A,G] := |Pr[ A(G(k))=1 ] - Pr[ A(r)=1 ]|, which always results in a value in the range [0,1], where k <-R- K, and r <-R- {0,1}^n`
- (In English: probability of PRG = 1 minus probability of true RN =1)
- If Adv is close to 1, then A can distinguish G from rand.
- If Adv is close to 0, then A cannot distinguish G from rand.

A silly example:
- If `A(x)=0`, always, then `AdvPRG[A,G]=0`.
- [I expected this to equal 1.  Why is Pr[A(r)=1]=0? I expected A(TRG)=1.]

A more interesting example:
- Suppose `G: K --> {0,1}^n satisfies msb(G(k))=1  for 2/3 of keys in K`
- Define stat. test A(x) = if [ msb(x)=1 ] output 1 else output 0.
- Then `AdvPRG[A,G} = |Pr[A(G(k))=1] - Pr[A(r)=1]|  = 2/3 - 1/2  =1/6`
- So, A breaks G with advantage 1/6.

### Secure PRG

Def: `G: K --> {0,1}^n` is a "secure PRG" if
- For all *efficient* statistical tests A:
  - `AdvPRG[A,G]` is negligible.
- Note: If we were to remove the "efficient" requirement, then it would be impossible to satisfy the previous statement.

It is not possible to prove that a PRG is secure. It is unknown (If you could prove it, then that would imply that P != NP).  [** what does this mean?]

Even though we cannot prove security, there are lots of good heuristic candidates.

### Unpredictable PRG

A secure PRG is necessarily unpredictable:
- Define statistical test B as:  `B(x) =`
  - `if A(bits 1 through i of x) =  bit i+1 of x, then output 1`
  - `else output 0`
- `r <-R- {0,1}^n: Pr[B(r)=1] = 1/2`
- `k <-R- K: Pr[B(G(k))=1] > 1/2 + e`
- Therefore, `AdvPRG[B,G} > e`

An unpredictable PRG is secure:
- Let `G: K --> {0,1}^n` be PRG
- Thm (Yao 1982): if for all i in {0, ..., n-1} PRG G is unpredictable at posn i then G is a secure PRG.
- If next-bit predictors cannot distinguish G from random then *no* statistical test can.

Generally:
- Let P1 and P2 be two distributions over {0,1}^n
- Def: We say that P1 and P2 are "computationally indistinguishable"
  - (denoted P1 ~=p P2, where ~=p means "in polynomial time they are computationally indistinguishable")
  - if for all eff. stat. tests A: 
  - `|Pr[A(x)=1] - Pr[A(y)=1]| < negligible`
  - Where `x <- P1` and `y <- P2`
- Example: a PRG is secure if `{k <-R- K: G(k)} ~=p uniform({0,1}^n)`


## Week 1.11 (Lesson 2.6): Semantic Security

### What is a secure cipher?

Security is defined in terms of:
- What the attacker can do. (Attacker ability)
- What the attacker is trying to do. (Security requirement)

Example:
- Ability: obtains a single ciphertext
- Reqt: ciphertext should reveal no "info" about plaintext
- Remember Shannon definition of perfect secrecy:
  - `For all m_0,m_1 in M where |m_0| = |m_1|, {E(k,m_0)} = {E(k,m_1)}, where k <-- K`
  - As mentioned before this is unattainable, so we loosen it.
- Modify Shannon defn to be just computationally indistinguishable:
  - `For all m_0,m_1 in M where |m_0| = |m_1|, {E(k,m_0)} ~=_p {E(k,m_1)}, where k <-- K`
  - But this definition is also unattainable, so loosen it more.
- Modify to apply only to m0 and m1 that attacker can exhibit:
  - `For m_0,m_1 in M that the attacker can actually exhibit, where |m_0| = |m_1|, {E(k,m_0)} ~=_p {E(k,m_1)}, where k <-- K`

### Semantic Security for a one-time key

For experiments EXP(b), where b=0,1:
- Adversary sends m0 and m1 to Challenger.
- Challenger encrypts mb (either m0 or m1): `E(k,m_b) -> c`.
- Challenger sends c to Adversary.
- Adverary tries to guess if c was from `m_0` or `m_1`.
- For b=0,1: `W_b` := [ event that EXP(b)=1 ]
- `Adv_SS[A,E] := |Pr[W_0] - Pr[W_1]| is in [0,1]`
- (If Adv_SS was 0, then adversary was not able to distinguish the two.)
- (If Adv_SS was 1, then adversary was able to distinguish the two.)
- "Adv_SS[m,n]" means "semantic security advantage of m over n".
- So, `Pr[EXP(x)=y]` means "probability that the Challenger chose x and Adversary chose y in the experiment".

Def: E is semantically secure if for all "efficient" A
- `Adv_SS[A,E]` is "negligible".

Example of insecure encryption:
- Suppose efficient A can always deduce LSB of PT from CT.
- `E=(E,D)` is not semantically secure.
- `Adv_SS[B,E] = |Pr[EXP(0)=1] - Pr[EXP(1)=1]|  = |0-1|  =1`

Example of OTP being secure:
- c is either (k xor m0) or (k xor m1)
- `Adv_SS[A,E] = |Pr[A(k xor m_0)=1] - Pr[A(k xor m_1)=1]|`
- The property of xor is such that the distribution of (`k xor m_0`) and (`k xor m_1`) are the same.
- So OTP is semantically secure. Furthermore, it is SS for *all* A because the distributions are the same.


## Week 1.12 (Lesson 2.7): Stream ciphers are semantically secure

We are going to prove that when `G: K --> {0,1}^n` is a secure PRG, then stream cipher E, which is based on G, is semantically secure.
- For all semantically secure adversaries A, 
- there exists a PRG adversary st
- `Adv_SS[A,E] <= 2 . Adv_PRG[B,G]`
- (both sides of this equation are negligible)

Proof:
- We will secretly substitute G with a TRG, so `c := m_b xor r` (instead of `m_b xor G(k)`).
- For b=0,1: W_b := [event that EXP(b)=1]
  - `Adv_SS[A,E] = |Pr[W_0] - Pr[W_1]|`
- for b=0,1: R_b := [event that EXP(b)=1, but with TRN]

[I do not understand this proof @ 7:00]


## Week 2.1 (Lesson 3.1): What are block ciphers


### Canonical examples

- 3DES ("triple-dez"): n=64 bits, k=168 bits.
- AES: n=128 bits, k=128, 192, 255 bits.
Where we input an n-bit block and it encrypts/decrypts into an n-bit block.
Also, a k-bit key is used to encrypt/decrypt.

### High-level description of a block cipher

1. Expand `k` into a series of "round keys": `k --> k_1, k_2, ... k_n`
2. Iterate repeatedly by applying  the "round function" `R` to the message `m`:
   - `R(k_1,m) -> m_1`
   - `R(k_2,m_1) -> m_2`
   - `...`
   - `R(k_n,m_(n-1)) -> m_n`

### Performance

|  Type  | Cipher     | Block/Key Size | Speed (MB/sec) |
|--------|------------|----------------|----------------|
| Stream | RC4        |                | 126            |
| Stream | Salsa20/12 |                | 643            |
| Stream | Sosemanuk  |                | 727            |
| Block  | 3DES       |   64/168       |  13            |
| Block  | AES        |  128/128       | 109            |

Block ciphers are much slower than stream ciphers, but they can do some things that stream ciphers cannot.

### Abstractly: PRPs and PRFs

PRF (Pseudo Random Function) is defined over (K,X,Y): `F: K x X --> Y`
st there exists "efficient" algorithm to evaluate F(k,x).
- Where K is keyspace, X is input space, Y is output space.

PRP (Pseudo Random Permutation) is defined over (K,Y): `E: K x X --> X`
Such that:
- Exists "efficient" _deterministic_ algorithm to evaluate `E(k,x)`
- The function `E(k, . )` is one-to-one
- Exists "efficient" inversion algorithm `D(k,y)`

PRP examples:
- 3DES: K x X --> X  where X={0,1}^64, K={0,1}^168
- AES:  K x X --> X  where K=X={0,1}^128

Functionally, any PRP is also a PRF when X=Y and is efficiently invertible:
- A PRP is a PRF that has more structure.
- A PRP is a PRF where the input space and output space are the same, so X=Y, and is efficiently invertible once you have the secret key `k`.
- In some sense a PRP is a special case of a PRF -- but that is not entirely accurate.

### Secure PRFs

The definition of a secure PRP/PRF also defines a secure block cipher.

Let `F: K x X --> Y` be a PRF
- Funs[X,Y]: the set of _all_ functions from X to Y
- S_F = { F(k, . ) st k is in K } is contained in Funs[X,Y]

Intuition: a PRF is *secure* if: A random function in Funs[X,Y] is *indistinguishable* from a random function in S_F.
- Size of Funs[X,Y] is |Y|^|X|   (which is enormous)
- Size of S_F is |K|

So for AES:
- Size of Funs[X,Y] is 2^(128 x 2^128)
- Size of S_F is 2^128

An easy application: PRF => PRG
- Let `F: K x {0,1}^n --> {0,1}^n` be a secure PRF.
- Then the following `G: K --> {0,1}^nt` is a secure PRG:
  - (where `t` is number of blocks and `n` is bits per block)
  - `G(k) = F(k,0) || F(k,1) || ... || F(k,t)`
  - This is called "counter mode"
  - Key property: parallelizable
- Why this is secure: Security falls directly from the PRF property.


## Week 2.2 (Lesson 3.2): DES - Data Encryption Standard

How the Feistel Network works:
- Given functions `f_1, ..., f_d: {0,1}^n --> {0,1}^n`
- (Note that these functions do not have to be invertible.)
- Goal: build invertible function `F: {0,1}^2n --> {0,1}^2n`
- R is the right-side data, L is the left-side data.
- L_i+1 = R_i
- R_i+1 = L_i xor f_1(R_i)
- where i = [0,d]

How to invert one iteration of the Feistel network:
- R_i = L_i+1
- L_i = f_i+1(L_i+1) xor R_i+1

When the Feistel Network is made into hardware, the encryption is the same as the decryption, but we just reverse R with L.

This is used in many block ciphers, but not AES.

Thm (Luby-Rackoff 1985):
- `f: K x {0,1}^n --> {0,1}^n` is a secure PRF.
- 3 rounds of Feistel `F: K^3 x {0,1}^2n -> {0,1}^2n` is a secure PRP.
- This converts a PRF into a PRP.
- `f_0 = f_0(k_0,R_0)`
[Why 3 rounds? Why not 2 rounds?]

DES is a 16 round Feistel Network.
- All functions are derived from a single function where different round keys are passed in as a parameter: `f_i(x) = F(k_i,x)`
- 64 bit input -> IP (initial permutation) -> Feistel -> IP^-1 -> 64 bit output
[Is IP even necessary?]

The function F(k_i,x):
- x is 32 bits
- k_i is 48 bits
- First, expand x from 32 bits to 48 bits using a fixed mapping.
- xor expanded x with k_i
- Then divide the result into 8 chunks of 6 bits each.
- Each 6 bit chunk is fed into an S-box, each S-box outputs 4 bits.
- The result is 8 x 4 = 32 bits total.
- Then feed the result into a permutaion that just moves the bits around.

S-boxes:
- The S-boxes are just a lookup table that maps 6 bit patterns to 4 bit patterns: `{0,1}^6 --> {0,1}^4`

Example of a bad S-box choice:
- If it is linear then it is not secure.
[** Do not understand @16:57]

Choosing the S-boxes and P-box:
- No output bit should be close to a linear function of the input bits.
  - Choosing S-boxes and P-box at random would result in an insecure block cipher (key recovery after about 2^24 outputs) because it ends up being somewhat close to a linear function.
- S-boxes are 4-to-1 maps.


## Week 2.3 (Lesson 3.3): Exhaustive Search Attacks

Goal: Find key `k` given some input and output pairs: `m_i, c_i = E(k,m_i), i=1,...,3`

Lemma: Suppose DES is an *ideal cipher*
- 2^56 random invertible functions
- then for all m,c there is at most *one* key `k` st  `c=DES(k,m)`
- with probability >= 1 - 1/256 ~= 99.5%

Proof:
- Pr[ there exists k' != k: c = DES(k,m) = DES(k',m) ] <= Sum for all k' in {0,1}^56  Pr[DES(k,m) = DES(k',m)] = 2^56 . 1/2^64 = 1/2^8 = 1/256

For 2 DES pairs the unicity probability ~= 1 - 1/2^71

For AES-128: given 2 input/output pairs, unicity probability ~= 1- 1/2^128

Two input/output pairs are enough for exhaustive key search.

### DES challenge

msg = "The unknown message is: XXXX..."

This was given to public:
- m1="The unkn"
- m2="own mess"
- m3="age is: "
- c1, c2, c3, ... cn

Goal: find k in {0,1}^56 st DES(k,m_i)=c_i for i=1,2,3

- In 1997 it took 3 months to search the keyspace using the Internet.
- In 1997 it took 3 days to using EFF machine, deep crack ($250k).
- In 1999 it took 22 hours combining Internet with deep crack.
- In 2006 it took 7 days using 120 FPGAs (only $10k)
- So, DES is dead because it is only 56 bits.

### Method 1: Triple-DES

For strengthening DES against exhaustive search.

- Let `E: K x M --> M` be a block cipher
- Define `3E: K^3 x M --> M as 3E((k1,k2,k3),m) = E(k1,D(k2,E(k3,m)))`
- For 3DES key-size = 3 . 56 = 168 bits, but is 3 times slower than DES.
- But there is a simple attack in time 2^118, but this is still secure because it is greater than 2^90.

### Why not double-DES?

- Define `2E((k1,k2),m) = E(k1,E(k2,m))`
- key length = 112 bits for 2DES

Meet-in-the-Middle Attack:
- `M=(m1,...,m10), C=(c1,...,c10)`
- Find `(k1,k2) st E(k1,E(k2,M))=C`
- Rewrite as: `E(k2,M) = D(k1,C)`

Step 1: build table with 2^56 rows

This is a table of all the *forward* calculations.

| k0=00...00 | E(k0,M) |
| k0=00...01 | E(k1,M) |
| k0=00...10 | E(k2,M) |
| ...        | ...     |
| k0=11...11 | E(kn,M) |

then sort table on 2nd column.

Time to create & sort table: 2^56 . log(2^56)

Step 2: for all k in {0,1}^56 do: test if D(k,C) is in 2nd column.
- Time to search for each decryption: 2^56 . log(2^56)
- Total time to crack 2DES: (2^56 . log(2^56)) + (2^56 . log(2^56)) <2^63
- Space ~= 2^56
- Note that this same attack on 3DES can be done in time 2^118  [how?]

### Method 2: DESX

This method defends against exhaustive search and is more efficient than 3DES, but does not defend against more subtle attacks, so it is not a standard.

- `E: K x M --> M` is a block cipher, where M={0,1}^n
- Define EX as: `EX((k1,k2,k3),m) = k1 xor E(k2, m xor k3)`
- For DESX: key-len = 64+56+64 = 184 bits
- There is an easy attack in time 2^(64+56) = 2^120
- Note that if you xor only m or only E(), then it adds no additional security. [why?]


## Week 2.4 (Lesson 3.4): More attacks on block ciphers

### Attacks on the implementation

#### Side channel attacks

Example: For smartcard implementation, measure *time* to do enc/dec, measure *power* for enc/dec.

Example: On a 2 core machine, run the encryption code on one core and the attack code on the second core -- both cores share the same cache.

#### Fault attacks

Overclock, warm up -- cause processor to malfunction.
Computation errors in the final round can expose the secret key k.

Do *not* invent your own ciphers.
And do *not* implement your own ciphers.

#### Linear and differential attacks

Given *many* input/output pairs, can we recover key in time less than exhaustive search?
- Let c=DES(k,m)
- Suppose for random k,m: `Pr[ (m[i1] xor ... xor m[ir]) xor (c[jj] xor ... xor c[jv]) = (k[l1] xor ... xor k[lu]) ]  = 1/2 + e`
- In English: Pr[ (subset of msg bits) xor (subset of CT bits) = (subset of key bits) ]  = 1/2 + e
- For DES, e = 1/2^21 ~= 0.0000000477 (because the 5th S-box is too linear)

How to use this relation in a linear attack:
- `Pr[ (m[i_1] xor ... xor m[i_r]) xor (c[j_j] xor ... xor c[j_v]) = (k[l_1] xor ... xor k[l_u]) ]  = 1/2 + e`
- Thm: given 1/e^2 random pairs (m, c=DES(k,m)) then
  - `k[l_1,...,l_u] = MAJ[ m[i_1,...,i_r] xor c[j_j,...,j_v] ]`
  - with prob. >= 97.7%
  - "MAJ" means majority of the time.
  - Can be done in 1/e^2 time.
- [Difficult to understand @7:50]

Example: linear attack on DES:
- For DES, e = 1/2^21 
  - with 2^42 inp/out pairs can find k[l_1,...,l_u] in time 2^42
- Roughly speaking: can find 14 key "bits" this way in time 2^42
- Next, brute force remaining 42 bits (56-14=42) in time 2^42
- Total attack time ~= 2^43  ( << 2^56 )  with 2^42 random inp/out pairs
- (whereas exhaustive search only requires 3 inp/out pairs)

Lesson:
- A tiny bit of linearity in S_5 leads to a 2^42 time attack
- Do *not* design your own ciphers!

#### Quantum attacks

Generic search problem:
- Let `f: X --> {0,1}` be a function (where outputs are mostly 0).
- Goal: find x in X st f(x)=1.

Classical computer: best generic algorithm time = O( |X| )

Quantum computer: time = O( |X|^(1/2) )
- x^(1/2) is sqrt(x)

Can quantum algorithms be built? Unknown.

Quantum exhaustive search:
- Given m, c=E(k,m)  define:
  - f(k) = 1  if E(k,m)=c
  - f(k) = 0  otherwise
- Grover: quantum computer can find k in time O( |K|^(1/2) )
- This means that quantum computers cannot do exhaustive searches on 256-bit block ciphers (AES-256).


## Week 2.5 (Lesson 3.5): The AES block cipher

In 2000 NIST chose Rijndael (RYNE-dale) as AES, designed in Belgium.
- key sizes: 128, 192, 256 bits.
  - The larger the key, security increases and it takes longer to compute.
- block size: 128 bits.
- AES is a substitution-permutation network (not Feistel)
- In Feistel, only half the bits change in each round.
- In subs-perm *all* bits change in each round.
  - For AES-128, data is broken apart into 16 byte arrays (in a 4x4 square).
  - Each round, xor input with round key.
  - run result through substitution layer (ByteSub).
  - then run through permutation layer (ShiftRow & MixColumn).
  - (repeat for 10 rounds)
  - (in the final round, MixColumn is not performed)
- Every layer (subs & perm) are reversible.
- Round keys are expanded from a 16 byte AES key to 176 bytes (10 rounds, but 11 round keys).

ByteSub:
- A 1 byte S-box. 256 byte table (easily computable so it is more compact because it does not need to be hard-coded -- for faster implementations just pre-compute).
- Apply the S-box to every byte in the 16-byte (4x4) square.

ShiftRows:
- Do not shift row 1.
- Shift row 2 left 1 position.
- Shift row 3 left 2 positions.
- Shift row 4 left 3 positions.

MixColumns:
- Apply a linear transformation to each column.

For performance you can choose to pre-compute:
- Round functions
- And/or S-boxes

AES was implemented in hardware (Intel Westmore & AMD Bulldozer):
- aesenc, aesenclast: does one round
- aeskeygenassist: key expansion
- 14x speed-up

### Two attacks exist for AES

Best key recovery attack:
- four times better than exhaustive search (which makes AES-128 equivalent to a 126-bit key)

Related key attack on AES-256:
- There is a weakness in the key expansion -- if you choose keys that are "related".
- Given 2^99 inp/out pairs from *four related keys* in AES-256 can recover keys in time ~=2^99.


## Week 2.6 (Lesson 3.6): Block ciphers from PRGs

Can we build a PRF from a PRG? (yes)
- Let `G: K --> K^2` be a secure PRG
- Define 1-bit PRF  `F: K x {0,1} --> K` as
  - `F(k, x in {0,1}) = G(k)[x]`
- Thm: If G is a secure PRG then F is a secure PRF

Can we build a PRF with a larger domain (than just 1 bit):
- Let `G: K --> K^2`
  - Define `G_1: K --> K^4` as `G_1(k) = G(G(k)[0]) || G(G(k)[1])`
- We get a 2-bit PRF:
  - `F(k, x in {0,1}^2) = G_1(k)[x]

Can extend more (GGM PRF):
- Let `G: K --> K^2`
- Define PRF `F: K x {0,1}^n --> K`
- For input `x = x_0 x_1 ... x_n-1 in {0,1}^n` do 
- But, GGM PRF is not used in practice because it is slow. Instead we use heuristic methods (like AES).
[Where is the variable x coming from?]

We can now build a secure PRP from a secure PRG by plugging the GGM PRF into the Luby-Rackoff theorem (the 3 cycle Feistel network).


## Week 2.7 (Lesson 4.1): Review: PRPs and PRFs

An example concrete assumption about AES:
- All 2^80 time algorithms A have  `Adv_PRP[A, AES] < 2^(-40)`

Consider the PRP:
- `E(k,x) = x xor k`
- Where key space `K={0,1}`, input space `X={0,1}`
- This is a secure PRP.
- Note that E contains two (reversible) functions:
  - 0->0 & 1->1
  - 0->1 & 1->0
- But is E also a secure PRF? (no)
  - Note that Funs[X,X] contains *four* functions:
    - 0->0 & 1->1
    - 0->1 & 1->0
    - 0->1 & 1->1
    - 0->0 & 1->0
  - Attacker A:
    - Query F() at x=0 and x=1
    - If f(0)=f(1) output 1, else 0
    - Adv_PRF[A,E] = |0 - 1/2| = 1/2
- PRF switching lemma:
  - Any secure PRP is also a secure PRF, if |X| is sufficiently large.

Lemma: Let E be a PRP over (K,X)
- Then for any q-query adversary A:
- `| Adv_PRF[A,E] - Adv_PRP[A,E] |  <  q^2 / 2|X|`
- Suppose |X| is large so that  q^2 / 2|X|  is negligible
- Then Adv_PRP[A,E] negligible --> Adv_PRF[A,E] negligible
[Where did the q^2 function come from?]

Suggestion:
- Do *not* think about the inner-workings of AES and 3DES
- Just assume that both are secure PRPs.


## Week 2.8 (Lesson 4.2): Modes of operation: one time key

One time keys:
- Adversary's power:
  - Adv sees only one ciphertext (one-time key)
- Adversary's goal:
  - Learn info about PT from CT (semantic security)

Example: ECB (electronic code book) - incorrect use of a PRP:
- This is a classic mistake.
- Break PT into multiple blocks: m1, m2, ...
- c1 = ECB(m1), c2 = ECB(m2), ...
- Problem: If m1=m2, then c1=c2.
- ECB is not secure for messages that contain *more than one block*.

ECB is not semantically secure:
- Adversary A sends two messages:
  - m0 = "HelloWorld"
  - m1 = "HelloHello"
- Challenger returns two blocks of CT:
  - c1 = E(k,m[1])
  - c2 = E(k,m[2])
  - where m is randomly either m0 or m1.
  - and whre m[1] is the first half of m and m[2] is the second half of m.
- Adversary A:
  - If c1=c2 then output 0, else output 1  [should these outputs be reversed?]
  - Adv_SS[A,ECB] = 1

How to correctly use block ciphers to encrypt long messags:
- Deterministic counter mode from a PRF F:
- `E_DETCTR(k,m) = c_i = m_i xor f(k,i)  for i=0..l`
- Counter mode is a stream cipher built from a PRF.


## Week 2.9 (Lesson 4.3): Security for many-time key

Example uses:
- File systems: Same AES key for many files.
- IPsec: Same AES key for many packets.

Semantic security for many-time key:
- Adversary's power: CPA (chosen-plaintext attack)
  - Can obtain CT of arbitrary PT of his choice.
  - This is a conservative modeling of real life
- Adversary's goal: Break semantic security
- In a CPA:
  - the Adversary submits m0=m1
  - the Adversary has the ability to repeat the experiment many times

Up to this point all ciphers have been insecure under CPA.

### Solutions

Solution 1: Randomized encryption
- E(k,m) is a randomized algorithm
- Encrypting same msg twice gives different ciphertexts
- Ciphertext will always be longer than plaintext
- Roughly speaking: CT_size = PT_size + numRandomBits
- Increased CT size could be a problem for short messages
- Use a random number that *never* repeats whp (with high probability).

Solution 2: Nonce-based encryption
- A nonce (`n`) changes from msg to msg and is a public value.
- The nonce only needs to be unique, not random.
- For c = E(k,m,n), the (k,n) pair must *never* be used more than once.
- m = D(k,c,n)
- Nonce methods:
  - Nonce is a counter.
    - If decryptor has the same state, then it is not necessary to send nonce.
  - Encryptor chooses a random nonce.
    - Advantage: sender does not need to maintain state.
    - This approach is required when encrypting with same key from multiple devices (this is called "stateless" encryption).

### CPA security for nonce-based encryption

System should be secure when Adversary chooses nonces, but each nonce is required to be unique.
- Adversary sends n_i, m_i,0 and m_i,1 where |m_i,0| = |m_i,1|
- Def: nonce-based encryption is sem. sec. under CPA if for all efficient A:
- Adv_nCPA[A,E] =  | Pr[EXP(0)=1] - Pr[EXP(1)=1] |   is negligible.


## Week 2.10 (Lesson 4.4): Modes of operation: many time key (CBC)

### Construction 1: CBC with random IV

CBC is "cipher block chaining".
IV is "initialization vector".

- Let (E,D) be a PRP.
- E_CBC(k,m): choose *random* IV in X and do:
  - `IV chosen at random for every message`  [Why must IV be random?]
  - `c[0] = E(k, IV xor m[0])`
  - `c[1] = E(k, c[0] xor m[1])`
  - `c[2] = E(k, c[1] xor m[2])`
  - where the size of IV is exactly the size of one block
  - The final CT is: `[ IV, c[0], c[1], c[2] ]`
  - Note that the CT is one block longer than PT.
  - To decrypt: `m[0] = D(k, c[0]) xor IV`
- One corrupted block of CT affects two blocks of PT when decrypting.

Prove that CBC is semantically secure under CPA analysis:
- For any L>0,
- If E is a secure PRP (block cipher) over (K,X) then
- E_CBC is sem. sec. under CPA over (K, X^L, X^(L+1))
  - Takes messages of length L
  - and outputs CT of length L+1
- In particular, for a q-query adversary A attacking E_CBC
- there exists a PRP adversary B st:
  - `Adv_CPA[A,E_CBC] <= 2 . Adv_PRP[B,E] + (2 . q^2 . L^2) / |X|`
  - This is called the error term: `(2 . q^2 . L^2) / |X|`
  - In order for Adv_CPA to be negligible, then *both* Adv_PRP *and* the error term must be negligible.
- Note: CBC is only secure as long as  `q^2 . L^2  <<  |X|`
- q is the number of times we used k to encrypt messages.
- L is length of longest message.

Example:
- Suppose we want `Adv_CPA[A,E_CBC] <= 1/2^32`
  - Because `q^2 . L^2 / |X|  <  1/2^32`  [how do we know this?]
- For AES:
  - `|X| = 2^128`
  - so therefore `q . L < 2^48`
  - so this means that after 2^48 AES blocks, we *must* change key
- For 3DES:
  - `|X| = 2^64`
  - so therefore `q . L < 2^16`
  - so this means that after 2^16 3DES blocks, we *must* change key

Common mistake when using CBC with a random IV:
- It is *not* sufficient for IV to be distinct (unique). IV *must* be random.
- A CBC where attacker can *predict* the IV is not CPA-secure!!
- This bug exists in SSL/TLS 1.1: IV for record i is last CT block of record i-1
  - The function interface is misleading in SSL because it assumes the caller will take care of the randomization of IV before inputting it into the function.

### Construction 1': nonce-based CBC

- CBC with *unique* nonce (that is not random): key = (k,k_1)
- unique nonce means: (key, n) pair is used for only one message
  - `IV = E(k_1, nonce)`  [Why must the nonce use a different key than the msg?]
    - Note that k_1 != k
  - `c[0] = E(k, IV xor m[0])`
  - `c[1] = E(k, c[0] xor m[1])`
  - `c[2] = E(k, c[1] xor m[2])`

Lesson: Nonce must be encrypted before using it as the IV values. Also, the encryption of the nonce must be with a key different from what was used to encrypt the blocks of PT.

CBC technicality:
- When PT is not an even multiple of the block size, you must pad the final PT block.
- Ex: With TLS: for n>0, the pad is the number n repeated n times.
- This means that if the PT is an even multiple of the block size, then we must add an additional dummyblock containing nothing but pad info. So for TLS the final block would have 16 repeated 16 times.
- Note that you can avoid adding an extra dummy block with the technique called CBC with ciphertext stealing


## Week 2.11 (Lesson 4.5): Modes of operation: many time key (CTR)

### Construction 2: Randomized Counter Mode

Let F be a secure PRF: `F: K x {0,1}^n --> {0,1}^n`
- `IV chosen at random for every message`
- `c[0] = F(k, IV+0) xor m[0]`
- `c[1] = F(k, IV+1) xor m[1]`
- `c[2] = F(k, IV+2) xor m[2]`
- Counter starts at zero for m[0], increments to 1 form m[1], etc.
- Final CT includes IV.
- Note: parallelizable, unlike CBC.
- One corrupted block of CT affects one block of PT during decryption.

### Construction 2': Nonce Counter Mode

- IV is not truly random.
- `IV = 64-bit Sequential Nonce || 64-bit counter that `
  - Sequential Nonce starts at zero for every _? [When does nonce increment?]
  - Counter starts at 0 for every message.
- `c[0] = F(k, IV) xor m[0]`
- `c[1] = F(k, IV+1) xor m[1]`
- `c[2] = F(k, IV+2) xor m[2]`
- CT includes IV.  [Because IV is revealed, can't they predict next msg?]
- You cannot have more than 2^64 blocks in a single message.

### Randomized IV Counter-Mode: CPA analysis

- Counter-mode Thm: For any L>0,
- If F is a secure PRF over (K,X,X) then
- E_CTR is sem. sec. under CPA over (K,X^L,X^(L+1))
  - Encrypts PT that is L blocks long.
  - Produces CT that is L+1 blocks long.
- In particular, for a q-query adversary A attacking E_CTR
- there exists a PRF adversary B st:
- `Adv_CPA[A,E_CTR] <= 2 . Adv_PRF[B,F] +  2 . q^2 . L / |X|`
- Note: ctr-mode only secure as long as `q^2 . L << |X|` Better than CBC!

An example:
- `Adv_CPA[A,E_CTR] <= 2 . Adv_PRF[B,F] +  2 . q^2 . L / |X|`
- q = number of messages encypted with k
- L = length of longest message
- Suppose we want `Adv_CPA[A,E_CTR] <= 1/2^32`
- then `q^2 . L / |X| < 1/2^32`
- AES: `|X| = 2^128` and `q . L^(1/2) < 2^48`
  - So after 2^32 CTs each of len 2^32, must change key
  - (total of 2^64 AES blocks, less than CBC)

### Comparison of ctr mode vs CBC

| Trait                     | CBC            | Counter Mode |
|---------------------------|----------------|--------------|
| Uses                      | PRP            | PRF          |
| Parallel Processing       | No             | Yes          |
| Security of rand. enc.    | q^2 L^2 << |X| | q^2 L << |X| |
| Dummy padding block       | Yes/No         | N/A          |
| 1 byte msgs (nonce-based) | 16x expansion  | No expansion |

(for CBC, dummy padding block can be avoided using ciphertext stealing)

### Summary

- PRPs and PRFs: A useful abstraction of block ciphers.
- Two security (against eavesdropping) notions:
  - Semantic security against one-time CPA.
  - Semantic security against many-time CPA.
  - Note: Neither mode ensures data integrity.
- Stated security results summarized in the following table:

|Goal \ Power| One-Time Key   | Many-Time Key (CPA) | CPA and integrity  |
|------------|----------------|---------------------|--------------------|
| Semantic   | Stream Ciphers | Random CBC          | (Will cover later) |
|  Security  |  or            |  or                 |                    |
|            | Deterministic  | Random Counter-Mode |                    |
|            |  Counter-Mode  |                     |                    |


## Week 3 (Lesson 5.1): Message Authentication Codes

For now, we will try to provide *integrity* without confidentiality.

Examples:
- Protect public binaries on a disk.
- Protect banner ads on web pages.

MACs (Message Authentication Codes):
- The MAC signing algorithm, `S` generates a tag: `t <-- S(k,m)`
- The tag is very short even though the message is long.
- Alice sends Bob m and t.
- Bob verifies the message with the MAC verification algorithm, `V`: 
  - `V(k,m,t)` = yes or no
- Def: MAC I=(S,V) defined over (K,M,T) is a pair of algos:
  - S(k,m) outputs t in T (the tagspace)
  - V(k,m,t) outputs 'yes' or 'no'
  - For all k in K and for all m in M: `V(k,m,S(k,m)) = yes`
- Integrity requires a secret key
  - Attacker can easily modify message m and re-compute tag t.
  - So, for example, CRC (which has no key) is designed to detect random, not malicious, errors.

Secure MACs:
- Attacker's power: chosen message attack
  - for m_1,m_2,...,m_q attacker is given `t_i <-- S(k,m_i)`
- Attacker goal: existential forgery
  - produce some *new* valid message/tag pair (m,t).
  - `(m,t) is not in {(m_1,t_1),...,(m_q,t_q)}`
  - Attacker cannot produce a valid tag for a new message
  - Given (m,t) attacker cannot even produce (m,t') for t' != t.

Secure MAC game: 
- Adversary A sends m_1, which is in M, to Challenger.
- Challenger chooses k from K.
- Challenger calculates t_1 <-- S(k,m_1) and sends t_1 to Adversary.
- Adv. sends m_2 to Chal., and Chal. sends t_2, etc.
- So Adv. now knows the pairs (m_1,t_1), ..., (m_q,t_q)
- Then if Adv. sends a (m,t) pair
  - and Chal. calculates V(k,m,t)=yes
  - and (m,t) is not in {(m_1,t_1),...,(m_q,t_q)}
  - then Chal. returns b=1 (meaning that the MAC is not secure)
  - otherwise Chal. returns b=0 (meaning that MAC is secure)
- `Def: I=(S,V)` is a secure MAC if for all "efficient" A:
  - `Adv_MAC[A,I] = Pr[Chal. outputs 1]` is "negligible"
- Typically a tag length is 16, 96, or 128 bits

Example: Protect system files on disk:
- k is derived from user password (and does not store k on disk).
- At install time the system computes for each file: `t_i = S(k,F_i)`
- Later a virus modifies system files.
- User reboots into a clean OS and supplies his password
  - Then: secure MAC => all modified files will be detected
  - Note that virus could swap one file with another, unless the filename is also included in the MAC calculation.


## Week 3 (Lesson 5.2): MACs based on PRFs

Secure PRF ==> Secure MAC
- For a PRF `F: K x X --> Y` define a MAC `I_F = (S,V)` as:
  - `S(k,m) := F(k,m)`
  - V(k,m,t): output 'yes' if t = F(k,m) and 'no' otherwise.
- Thm: If` F: k x X --> Y` is a secure PRF and 1/|Y| is negligible
  - then I_F is a secure MAC
  - where |Y| = 2^80 for example.
  - Adv_MAC[A,I_F] <= Adv_PRF[B,F] + 1/|Y|

How to convert a small-MAC (ie |Y| is small) into a big-MAC? Two constructions:
- CBC-MAC (banking: ANSI X9.9, X9.19, FIPS 186-3)
- HMAC (Internet protocols: SSL, IPsec, SSH, ...)
- Both convert a small-PRF into a big-PRF.

A bad example:
- `F: K x X --> Y` is a secure PRF with `Y = {0,1}^10`
- Is the derived MAC I_F a secure MAC system?
  - No. Tag is too short (1024) possibilities.

Thm: If `F: K x X --> Y` is a secure PRF and 1/|Y| is negligible
- (i.e. |Y| is large) then I_F is a secure MAC.
- In particular, for every efficient MAC adversary A attacking I_F
- there exists an eff. PRF adversary B attacking F s.t.:
  - `Adv_MAC[A,I_F] <= Adv_PRF[B,F] + 1/|Y|`
  - (For the left side to be neg., both items on the right side must each be negligible also.)
- I_F is secure as long as |Y| is large, say |Y| = 2^80

Proof Sketch:
- Suppose `f: X --> Y` is a truly random function
- Then MAC adversary A must win the following game:
  - Adv. sends Chal. m_1 (which is in X)
  - Chal. sends Adv. t_1 <-- f(m_1)
  - Repeat, so Adv. now has m_1,...,m_q and t_1,...,t_q.
  - If Adv can create (m,t) where t=f(m) and m is not in {m_1,...,m_q} then Adv. wins.
  - Pr[Adv. wins] = 1/|Y|         same must hold for F(k,x)

Examples:
- AES: a MaC for 16-byte messages.
- Main question: how to convert Small-MAC into a Big-MAC?
- Two main constructions used in practice:
  - CBC-MAC (banking - ANSI X9.9, X9.19, FIPS 186-3)
  - HMAC (Internet protocols: SSL, IPsec, SSH, ...)
- Both convert a small-PRF into a big-PRF.

Truncating MACs based on PRFs:
- Easy lemma: suppose `F: K x X --> {0,1}^n` is a secure PRF.
  - Then so is `F_t(k,m) = F(k,m)[1...t]` for all 1 <= t <= n
  - (but 1/(2^t) must be negligible... see next few lines)
- If (S,V) is a MAC is based on a secure PRF outputting n-bit tags
- the truncated MAC outputting w bits is secure
- ...as long as 1/(2^w) is still negligible (say w >= 64)


## Lesson 5.3: CBC-MAC and NMAC

CBC-MAC and NMAC are used to turn small-MAC into big-MAC.

From now on, let X = {0,1}^n  (e.g. n=128 for AES)

Construction 1: ECBC (Encrypted CBC-MAC)
- Let `F: K x X --> X` be a PRP
- Define new PRF `F_ECBC: K^2 x X^(<=L) --> X`
  - (L is the number of blocks)
  - K^2 represents two keys as inputs
- ECBC process:
  - x_1 = F( k,m[1] )
  - x_2 = F( k,m[2] xor x_1 )
  - ...
  - x_L = F( k, m[L] xor x_L-1 )
    - x_L is called the "raw CBC" and is *not* a secure MAC.
    - (Intermediate values are not saved.)
  - Then encrypt the raw CBC with a second key: tag = F(k_1,x_L)

Construction 2: NMAC (Nested MAC)
- Let `F: K x X --> X` be a PRF
- Define new PRF `F_NMAC: K^2 x X^(<=L) --> K`
  - (L is the number of blocks)
  - K^2 represents two keys as inputs
- NMAC process:
  - k_1 = F( k, m[1] )
  - k_2 = F( k_1, m[2] )
  - ...
  - k_L = F( k_L-1, m[L] )
    - The above is called a "cascade function" and is *not* a secure MAC.
  - Then encrypt the result of the cascade function with another key:
    - tag = F( k, k_L || fpad )
  - [What is fpad?]
- The output of the cascade function is not secure because it can be forged with one chosen message query.

The final encryption in NMAC prevents an extension attack:
- Given the output of cascade(k,m)
- we can derive from it cascade(k, m||w) for any w

How to attack a raw CBC (without the final encryption):
- Suppose we define a MAC I_RAW = (S,V) where
- S(k,m) = rawCBC(k,m)
- Then I_RAW is easily broken using a 1-chosen msg attack.
  - Adv. chooses an arbitrary one-block msg: m which is in X
  - Adv. requests tag for m.  Gets t=F(k,m)
  - Adv. outputs t as MAC forgery for the 2-block message m'
    - where m' = [m, t xor m]
  - rawCBC( k, [m, t xor m] )
    - = F( k, F(k,m) xor (t xor m) )
    - = F(k, t xor (t xor m) )
    - = t

ECBC-MAC and NMAC analysis:
- Thm: For any L>0,
  - For every eff. q-query PRF adv. A attacking F_ECBC or F_NMAC
  - there exists an eff. adv. B s.t.:
    - `Adv_PRF[A,F_ECBC] <= Adv_PRP[B,F] + 2q^2 / |X|`
    - `Adv_PRF[A,F_NMAC] <= q . L . Adv_PRF[B,F] + q^2 / 2|K|`
  - CBC-MAC is secure as long as  q << |X|^(1/2)
  - NMAC is secure as long as  q << |K|^(1/2)    (2^64 for AES-128)

An example:
- `Adv_PRF[A,F_ECBC] <= Adv_PRP[B,F] + 2q^2 / |X|`
- q = number of messages MAC-ed with k
- Suppose we want `Adv_PRF[A,F_ECBC] <= 1/2^32`
  - In other words we want `q^2 / |X|  <  1/2^32`
- For AES:  |X| = 2^128  ==>  q < 2^48
  - So, for AES, after 2^48 messages, you *must* change the key
- For 3DES: |X| = 2^64  ==>  q < 2^16
  - So, for 3DES, after 2^16 messages, you *must* change the key

The security bounds are tight: an attack
- After signing |X|^(1/2) messages with ECBC-MAC
  - or |K|^(1/2) messages with NMAC
  - the MACs become insecure.
- Suppose the underlying PRF F is a PRP (e.g. AES)
  - Then both PRFs (ECBC and NMAC) have the following extension property:
    - For all x,y,w:  F_BIG(k,x) = F_BIG(k,y)
      - (in other words, a collision on messages x and y)
    - Then that also implies: F_BIG(k, x || w) = F_BIG(k, y || w)
      - (in other words, a collision on extensions of messages x and y)
- Generic attack on the derived MAC:
  - Step 1: Issue |Y|^(1/2) message queries for rand. messages in X.
    - (For AES we issue 2^64 message queries.)
    - Obtain (m_i, t_i)   for i=1, ..., |Y|^(1/2)
  - Step 2: Find a collision  t_u = t_v  for u != v
    - (one exists w.h.p by b-day paradox)
  - Step 3: Choose some w and query for  t := F_BIG(k, m_u || w)
  - Step 4: Output forgery (m_v || w, t)
    - Indeed  t := F_BIG(k, m_v || w)

ECBC-MAC is commonly used as an AES-based MAC
- CCM encryption mode (used in 802.11i)
- NIST standard called CMAC

NMAC not usually used with AES or 3DES
- Main reason: need to change AES key on every block
  - requires re-computing AES keys expansion
- But NMAC is the basis for a popular MAC called HMAC (next)


## Lesson 5.4: MAC padding

In the prior section we always assumed the message length was an even multiple of the block size.

Bad idea: Pad m with 0's.
Because pad(m) = pad(m || 0)    so m and m||0 have the same tag.
[Why is this so?]

CBC-MAC padding function must be invertible (it must be one-to-one)!
That is, two distinct messages must alway map to two distinct padded messages:
- When m_0 != m_1, then it must also be true that pad(m_0) != pad(m_1)
  - where pad(m) is the message m after having padding added.

ISO proposed padding function
- Pad with "1000...00"
  - The "1" indicates beginning of pad.
- If the message is an even multiple of the block size then you *must* add a dummy block containing "1000...0"
- If you forget the dummy block then the MAC becomes vulnerable to an easy existential forgery attack. This is possible because these two messages would have the same tag:
  - Unpadded msg: 1010 1100    Padded w/o dummy block: 1010 1100
  - Unpadded msg: 1010 1       Padded w dummy block:   1010 1100

CMAC has a built-in padding function (NIST standard)
- Avoids dummy block.
- Variant of CBC-MAC where   key = (k, k_1, k_2)
  - (k_1 and k_2 are actually derived from k.)
  - No final encryption step (extension attack thwarted by final keyed xor)
  - No dummy block (ambiguity resolved by use of k_1 or k_2)
- On the final block, if m is not an even multiple of the block length:
  - Append the ISO padding (1...0) to m[w]
  - Then calculate: m[w] xor k_1 xor F(k,.)
- On the final block, if m is an even multiple of the block length:
  - Do not pad.
  - Then calculate: m[w] xor k_2 xor F(k,.)


## Lesson 5.5: Message Integrity - A Parallel MAC

Construction 3: PMAC (parallel MAC)
- Let `F: K x X --> X` be a PRF
- Define new PRF `F_PMAC: K^2 x X^(<=L) --> X`
- For block i: output_i = F( k_1, m[i] xor P(k,i) )
  - Where i = 1..L-1
- For block L: output_L = m[L] xor P(k,L)
  - (for a technical reason it's not necessary to appy F() to block L.)
- Then tag = F( k_1, output_1 xor output_2 xor ... xor output_L )
- If P() did not exist then the MAC would be insecure because there would be no way to enforce the order of the blocks. P() is an easy to compute function.
- Padding is similar to CMAC.

PMAC Theorem:
- For any L>0,
  - If F is a secure PRF over (K,X,X) then
  - F_PMAC is a secure PRF over (K, X^(<=L), X).
- For every eff. q-query PRF adv. A attacking F_PMAC
  - there exists an eff. PRF adversary B S.T.:
    - `Adv_PRF[A,F_PMAC] <= Adv_PRF[B,F] +  2 . q^2 . L^2 / |X|
- PMAC is secure as long as qL << |X|^(1/2)
  - where q is the number of messages MAC-ed using a given key
  - L is the maximum length of all the messages
  - |X| is the block size.

PMAC is incremental (if F is a PRP)
- If one block is corrupted/faked, then it is very fast to recalculate the MAC because you only need to recalculate the bad block and xor it into the final encryption.

One time MAC (analog of one time pad)
- It is a MAC that uses one key per message.
- For a MAC  I=(S,V) and adv. A define a MAC game as:
  - Adv. sends to Chal.: m_1 which is in M
  - Chal. sends to Adv.: t_1 <--S(k,m_1)
  - Adv. sends to Chal.: Forged values (m,t)
  - Chal. returns:
    - b=1 if V(k,m,t) = yes and (m,t) != (m_1,t_1)
    - b=0 otherwise
- Def: I=(S,V) is a secure MAC if for all efficient A:
  - `Adv_MAC[A,I] = Pr[Chal. outputs 1] is negligible`

One time MAC Example
- Can be secure against *all* adversaries and faster than PRF-based MACs.
- Let q be a large prime (e.g. q = 2^128 + 51)
  - key = (k,a) is in {1,...,q}^2    (two random ints. in [1,q])
    - k is the 1st half of our secret key & a is the 2nd half.
  - msg = ( m{1}, ..., m[L] )   where each block is 128 bit int.
    - `S( key, msg ) = P_msg(k) + a  (mod q)`
  - where P_msg(x) = m[L] . x^L + ... + m[1] . x   is a poly. of deg L.
- Fact: Given S(key,msg_1)  adv. has no info about S(key,msg_2)

General technique to convert one-time MAC to many-time MAC
- Let (S,V) be a secure one-time MAC over ( K_I, M, {0,1}^n )
- Let F: K_F x {0,1}^n --> {0,1}^n  be a secure PRF.
- Carter-Wegman MAC: CW( (k_1,k_2), m )  =  ( r, F(k_1,r) xor S(k_2,m) )
  - F(k_1,r) is slow but short input
  - S(k_2,m) is fast but long input
  - for random nonce r <-- {0,1}^n
- Thm: If (S,V) is a secure one-time MAC and F a secure PRF
  - then CW is a secure MAC outputting tags in {0,1}^(2n)

Construction 4: HMAC (hash-MAC)
- Most widely used MAC on the Internet.


## Lesson 6.1: Collision Resistance Intro

HMAC is based on NMAC.

Define Collision Resistance
- Let  H: M --> T  be a hash function  ( |M| >> |T| )
- A collision for H is a pair m_0, m_1 which are in M s.t.:
  - H(m_0) = H(m_1)  and  m_0 != m_1
- A function H is collision resistant if for all (explicit) "eff" algs. A:
  - `Adv_CR[A,H] = Pr[A outputs collision for H]` is "neg".
- Example: SHA-256 (outputs 256 bits)
- (The "explicit" efficiency requirement is important -- this means we have to be able to actually write down the algorithm, not just theorize it.)

MACs from Collision Resistance
- Let  I = (S,V)  be a MAC for short messages over (K,M,T)  (e.g. AES)
- Let  H: M^big --> M
- Def:  I^big = (S^big, V^big)  over (K, M^big, T) as:
  - `S^big(k,m) = S(k,H(m)) ; V^big(k,m,t) = V(k,H(m),t)`
- Thm: If I is a secure MAC and H is collision resistant
  - then  I^big  is a secure MAC.
- Example: S(k,m) = AES_2-block-cbc(k, SHA-256(m))  is a secure MAC.
- Collision resistance is necessary for security:
  - Suppose adversary can find  m_0 != m_1  s.t.  H(m_0)=H(m_1).
  - Then: S^big is insecure under a 1-chosen msg attack
    - step 1: adversary asks for  t <-- S(k,m_0)
    - step 2: output  (m_1, t) as forgery
  - All it takes is one collision to prove it is not secure.

Protecting file integrity using collision resistant hash
- When downloading files off the Internet, need a read-only space that stores hashes of publicly downloadable files.
- No key needed.


