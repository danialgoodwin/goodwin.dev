---
title: How to write Fermat's factorization method in Kotlin
tags:
- kotlin
- prime numbers
---

I recently learned about how Fermat's factorization method was used to efficiently factorize some RSA public keys.

Basically, the RSA public key is a semiprime, meaning that its only factors are two (different) prime numbers. And, if these prime numbers are close to each other, then Fermat's factorization method can quickly find the factors. The basic form of this factorization method is `(a - b) * (a + b)`, where `a-b` is one factor and `a+b` is the other factor.

Here's my commented code in a GitHub Gist for calculating the RSA factors using Kotlin:

<script src="https://gist.github.com/danialgoodwin/ff9437f1a92228918e57c6f569ec709b.js"></script>

- I'm using `BigInteger`s in my code because the numbers I'm factoring quickly pass the limits of `long`s. All RSA public keys are much larger than `long`.

Here's some times I benchmarked on my desktop:

    20-bit, 640361               , Factors: (397, 1613)                in 1 ms        (iterations: 204)
    30-bit, 903395833            , Factors: (14869, 60757)             in 5 ms        (iterations: 7756)
    40-bit, 923078151197         , Factors: (501821, 1839457)          in 34 ms       (iterations: 209869)
    50-bit, 769817083110377      , Factors: (13685611, 56250107)       in 724 ms      (iterations: 7222281)
    60-bit, 777571995069352537   , Factors: (427381733, 1819385189)    in 27626 ms    (iterations: 241583032)
    70-bit, 765702222853455251363, - Likely a larger jump in time because its past 64-bits that fit into a `long`, so another path in BigInteger is used
    80-bit, 1105986106596451832095997,
    
    fun benchmark(count: Int, task: () -> Unit): Long {
        val times = mutableListOf<Long>()
        for (i in 0..count) {
            val start = System.currentTimeMillis()
            task.invoke()
            if (i != 0) times.add(System.currentTimeMillis() - start)
        }
        return times.sum() / count
    }

Some ideas for future performance enhancements:
- Read existing publications on prime factorization, like the 'quadratic sieve' and 'general number field sieve'.
- Use co-routines (multiple threads), but that's only a linear improvement
- Create a custom `java.math.MutableBigInteger`. `BigInteger` creates a copy on many operations.
- Find out exactly what operations are exponential and improve that
 

    fun BigInteger.isSquare_ideas(): Boolean {
        // Idea 1:
        return this.sqrtAndRemainder()[1] == BigInteger.ZERO
        // Idea 2:
        // val bits = this.bitLength()
        // for (i in 0 until bits) {
        //     if (this.testBit(i)) return false
        // }
        // return true
        // Idea 3:
        // return this.lowestSetBit == this.bitLength() - 1
    }

To learn more about Fermat's factorization method, see Wikipedia: [https://en.wikipedia.org/wiki/Fermat%27s_factorization_method](wikipedia.org/wiki/Fermat%27s_factorization_method)
