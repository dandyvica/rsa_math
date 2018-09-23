# rsa_math

RSA algorithms, private/public key, certificates, etc

# Mathematical background

## Whitfield Diffie and Martin Hellman

* _New Directions in Cryptography_ (1976)
* concepts: public/private key for encryption and digital signature

## Math facts

* finding a large prime (large is > 512 bits or 155 digits) is easy
* multiplying 2 large integers is easy
* factoring a large integer is hard
* modular exponentiation is easy: given $n,m$ and $e$ it's easy to compute $c = m^e \; (mod \; n)$
* modular root extraction (the reverse of modular exponentiation) is easy given the prime factors: given $n,c$ and $e$ and prime factors $p$ and $q$, it's easy to recover the value $m$ such that $c = m^e \; (mod \; n)$. Modular root extraction is otherwise hard

## RSA algorithm

1. Generate a pair of large, random primes $p$ and $q$
1. Compute the modulus $n$ as $n = pq$
1. Select an odd public exponent $e$ between 3 and $n-1$ that is coprime to $\;\phi(n) = (p-1)(q-1)$
1. Compute the private exponent $d$ from $e$, $p$ and $q$ such that $d$ is the multiplicative inverse of $e$: $\;e.d \equiv 1 \; (mod \;\phi(n))$ which means, for some $k$: $\quad e.d \; = 1 + k\phi(n)$
1. Output $(n, e)$ as the public key and $(n, d)$ as the private key

### Encrypting plain text $M$ to get cyphertext $C$

* Now if $M$ is the plaintext: $\quad C \equiv M^e \; (mod \; n)$

### Decrypting cypher text $C$

* Then: $\quad C^d \equiv (M^e)^d \equiv M^{ed} \equiv M^{1+k\phi(n)} \equiv M.M^{k\phi(n)} \; (mod \; n)$

* But applying Euler's theorem: 
- $\quad M^{\phi(p)} = M^{p-1} \equiv  1 \; (mod \; p)$
- $\quad M^{\phi(q)} = M^{q-1} \equiv  1 \; (mod \; q)$

* and using basic congurences properties:
- $\quad (M^{p-1})^{k(q-1)} = M^{k(p-1)(q-1)} \equiv  1 \; (mod \; p)$
- $\quad (M^{q-1})^{k(p-1)} = M^{k(p-1)(q-1)} \equiv  1 \; (mod \; q)$

* and using Chinese remainder theorem: $M^{k\phi(n)} \equiv 1 \quad (mod \; n)$
* so: $\quad C^d \equiv M \; (mod \; n)$


## Implementation concepts

* m is the plaintext message (ascii string)
* convert ASCII chars to hexadecimal values and into a 10-base integer

# OpenSSL

## Step by step example

_openssl_ can be used to generate cryptographic ecosystem. The following examples use a small key of 32 bits, which is the smallest key possible (actually 31?).

* generate 32-bit RSA keys (all above values like n, p, a, ...)

```zsh
openssl genrsa -out key32.pem 32
```

The _key32.pem_ PEM file is the standard for holding RSA keys, which is a Base64-encoded version of all the RSA keys.

```text                                   
-----BEGIN RSA PRIVATE KEY-----
MC0CAQACBQDOfarNAgMBAAECBClWIwECAwDtawIDAN6nAgMAhI0CAj93AgMAofw=
-----END RSA PRIVATE KEY-----
```

This file has all the RSA keys into a single file. We can view individual elements of the RSA keys:

```zsh
openssl rsa -text -in key32.pem
Private-Key: (32 bit)
modulus: 3464342221 (0xce7daacd)
publicExponent: 65537 (0x10001)
privateExponent: 693510913 (0x29562301)
prime1: 60779 (0xed6b)
prime2: 56999 (0xdea7)
exponent1: 33933 (0x848d)
exponent2: 16247 (0x3f77)
coefficient: 41468 (0xa1fc)
writing RSA key
-----BEGIN RSA PRIVATE KEY-----
MC0CAQACBQDOfarNAgMBAAECBClWIwECAwDtawIDAN6nAgMAhI0CAj93AgMAofw=
-----END RSA PRIVATE KEY-----
```

This includes the modulus (also referred to as public key and n), public exponent (also referred to as e and exponent; default value is 65537), private exponent (d), and primes used to create keys (prime1, also called p, and prime2, also called q).

We can verify n:

* 3464342221 == 60779 * 56999 (n == pq)




