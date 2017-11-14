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
* modular exponentiation is easy
* modular root extraction (the reverse of modular exponentiation) is easy given the prime factors
* modular root extraction is otherwise hard

## RSA algorithm

1. Generate a pair of large, random primes $p$ and $q$
1. Compute the modulus n as n = pq
1. Select an odd public exponent e between 3 and n-1 that is relatively prime to p-1
and q-1
1. Compute the private exponent d from e, p and q
1. Output (n, e) as the public key and (n, d) as the private key

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
```

This includes the modulus (also referred to as public key and n), public exponent (also referred to as e and exponent; default value is 65537), private exponent (d), and primes used to create keys (prime1, also called p, and prime2, also called q).

We can verify value:

* 3464342221 == 60779 * 56999 (n == pq)




