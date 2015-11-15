# 350 - All Zob's Fault

#### Overview

Factor an low-bit RSA modulus and decrypt the ciphertext to attain the flag.

#### Problem

Apparently our crypto program didn't store the correct phi-value or something for this message; it spits out gibberish when we try to decrypt it. Either the memory had a rare fault or Zob's being an idiot again. Please help.

    e = 340035333160956238336074318075946961695270890880263371398510321485728225; 
    m = 614010459838253953596498114943057697842675637887066261109163514805589167; 
    c = 416808431213057812839807235099929401146034654633863359116938353620975451;

#### Solution

There are two ways to start this problem.  One would be to believe that the memory or message truly had a rare fault, or to believe that Zob was just being stupid.  If you chose the former, you would quickly get stuck.  On the other hand, if you chose the latter option - to believe that Zob had no idae what he was doing when he encrypted his message, then you just solved half of the problem.  One may ask: "What does it mean when Zob is being an idiot?"  An average person would logically assume that m stands for modulus.  Maybe that's what Zob did wrong.  Unfortunately for us, we have to factor the assumed to be modulus in order to attain its individual primes, which are required to calculate the totient and private key.  Normally, this would be impossible, but since the modulus is rather small for RSA, it can be done.  After a bit of browsing, I found factordb.com, which can find the prime factors of a 100 digit number in a fraction of a second.    This seems useful for our purposes.  We enter the modulus into the website, and it factors!

The primes are 

    783420406144696097385833069281677113 and 783756020423148789078921701951691559.

#### Flag

TODO
