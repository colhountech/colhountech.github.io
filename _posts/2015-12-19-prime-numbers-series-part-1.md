---
title: Prime Numbers Series - Part 1: Simple Prime Number Search
layout: post
published: true
---

This is part 1 of a series of posts on writing scalable software.

# Start at the Beginning.

So, what are the rules for finding prime numbers:

* Rule 1: the number is greater than 1
* Rule 2: the number is only divisible by 1 and itself.

Here is probably the simplest approach to implementing this in code, and is the same approach used to prove many hypotheses. We assume the number is a prime, and scan through all cases, apply the rule to each case if the rule fails we mark the candidate as being not a prime number. If the hypothesis passes all rules at the end, we have found a prime.

Here is the c# code:

        static void Main(string[] args)
        {
            long candidate = 1024 * 1024 * 1024 + 3;
            Stopwatch watch = Stopwatch.StartNew();
            long startAt = 2;
            bool isPrime = true;
            for (long i = startAt; i < candidate; i++)
            {
                if (candidate % i == 0)
                {
                    isPrime = false;
                    break;
                }
            }
            if (isPrime)
            { 
                Console.Write("Found a new prime: {0}\n", candidate);
            }
            Console.WriteLine("Time Taken = {0:n} msec", watch.ElapsedMilliseconds);
        }
		
              	
You should get the following output:

	Found a new prime: 1073741827
	Time Taken = 16,916.00 msec

Let's optimise our algorithm slightly. A non-prime number p, can always be refactored to n x m. But if sqrt(p) = n x n, so one of these must be  less than sqrt(p) or (n x m) would be greater than or equal to p, so we only need to check  for all values less than sqrt (p).

Our new algorithm:

        static void Main(string[] args)
        {
            long candidate = 1024 * 1024 * 1024 + 3;
            Stopwatch watch = Stopwatch.StartNew();
            long startAt = 2;
            bool isPrime = true;
            for (long i = startAt; i < Math.Sqrt(candidate); i++)
            {
                if (candidate % i == 0)
                {
                    isPrime = false;
                    break;
                }
            }
            if (isPrime)
            {
                Console.Write("Found a new prime: {0}\n", candidate);
            }
            Console.WriteLine("Time Taken = {0:n} msec", watch.ElapsedMilliseconds);
        }
			
You should get the following output:

	Found a new prime: 1073741827
	Time Taken = 3 msec
	
Wow. That's fast. 

Let's try a bigger prime: how about long.MAXVALUE - 24, or 9,223,372,036,854,775,783

Found a new prime: 9,223,372,036,854,775,783
Time Taken = 146493 msec to scan 9,223,372,036,854,775,783

We are starting to hit more limits now.

Firstly, the time taken is getting long again. Over two minutes, but if you look at our CPU usage, it's stuck solid on 50% cpu utilisation. Since we are running a single linear thread of execution in our candidate search, we are not making full use of the processor cores in the computer. Maybe we should consider running our search in parallel over several threads of execution. We will look at this in a later post.

 Secondly, our "long" variable can't hold a number larger than MAXVALUE so 9223372036854775783 is the largest prime we can search for.

Let's see if we can look for bigger number. I came across a library recently called System.Numerics which can deal with big integers. Let's give it a spin.

Before we start looking for primes larger that 9223372036854775783, let's search for our first test prime of 
so that we can benchmark the difference between a long search and a BigInteger search.

There are a few new concepts here. Firstly, there is no Math.Sqrt() for BigInteger, so we had to roll our own. Here is a sample on getting the Sqrt of a BigInteger. I have implemented this as an extension method. This is derived from the following stackoverflow post with a few tweaks. Here is the code to calculate the sqrt:


          public static BigInteger Sqrt(this BigInteger n)
            {
                if (n == 0) return 0;
                if (n < long.MaxValue)
                {
                    return  (BigInteger) Math.Sqrt((long)n);
                }
                if (n > 0)
                {
                    int bitLength = Convert.ToInt32(Math.Ceiling(BigInteger.Log(n, 2)));
                    BigInteger root = BigInteger.One << (bitLength / 2);
                    while (!isSqrt(n, root))
                    {
                        root += n / root;
                        root /= 2;
                    }
        
                    return root;
                }
        
                throw new ArithmeticException("NaN");
            }
        
            private static Boolean isSqrt(BigInteger n, BigInteger root)
            {
                BigInteger lowerBound = root * root;
                BigInteger upperBound = (root + 1) * (root + 1);
        
                return (n >= lowerBound && n < upperBound);
            }

and I have moved the prime search to this extensions method:

        public static bool IsPrime(this BigInteger candidate)
            {
                BigInteger sqrt = candidate.Sqrt();
                for (BigInteger i = 2; i < sqrt; i++)
                {
                    if (candidate % i == 0)
                    {
                        return false;
                    }
                }
                return true;
            }

Ouch! not good. 

        Found a new prime: 9,223,372,036,854,775,783
        Time Taken = 1,145,536 msec to scan 9,223,372,036,854,775,783

This is approx 8 times slower than the previous calculation, but gives us a much bigger range. I've run off the first 3 primes starting with (long.MAX_VALUE - 24);
        
        Found a new prime: 9,223,372,036,854,775,783. Time Taken = 1,690,790 msec
        Found a new prime: 9,223,372,036,854,775,837. Time Taken = 1,550,355 msec
        Found a new prime: 9,223,372,036,854,775,907. Time Taken = 1,884,315 msec
        Found a new prime: 9,223,372,036,854,775,931. Time Taken = 1,599,943 msec
        Found a new prime: 9,223,372,036,854,775,939. Time Taken = 1,718,325 msec
        Found a new prime: 9,223,372,036,854,775,963. Time Taken = 1,580,920 msec

Our next optimisation: We actually don't need to check every number, because if the number we are about to check is not a prime, it would already have been identified by one of it's divisors, so we actually only need to check that it can be divided by a lower prime number. So, a new algorithm would be to have a array of every number between 2 and our candidate, and flag this number as true if it's a prime, or false if it's not a prime, and as we build up each candidate, we can first check if this number exists in our array first. Let's see what happens if we try this.

Now, we quickly run out of memory if we do this, so what we can do is provide a service that caches the previous primes that we have persisted to storage, so that we don't have out of memory issues. That's our next step.
