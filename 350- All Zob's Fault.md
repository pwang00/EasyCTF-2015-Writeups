# 350 - All Zob's Fault

Note: this writeup assumes that you understand how the RSA cryptosystem works.  If not, head over to https://en.wikipedia.org/wiki/RSA_(cryptosystem) for a quick and beginner-friendly guide.

#### Overview

Factor an low-bit RSA modulus and decrypt the ciphertext to attain the flag.

#### Problem

Apparently our crypto program didn't store the correct phi-value or something for this message; it spits out gibberish when we try to decrypt it. Either the memory had a rare fault or Zob's being an idiot again. Please help.

    e = 340035333160956238336074318075946961695270890880263371398510321485728225; 
    m = 614010459838253953596498114943057697842675637887066261109163514805589167; 
    c = 416808431213057812839807235099929401146034654633863359116938353620975451;

#### Solution

There are two ways to start this problem.  One would be to believe that the memory or message truly had a rare fault, or to believe that Zob was just being stupid.  If you chose the former, you would quickly get stuck.  On the other hand, if you chose the latter option - to believe that Zob had no idea what he was doing when he encrypted his message, then congratulations, you are now one step closer to solving the problem.  One may ask: "What does it mean when Zob is being an idiot?"  It is likely that an average person would assume that m stands for modulus.  Maybe that's what Zob did wrong.  Unfortunately for us, we have to factor the assumed to be modulus in order to attain its individual primes, which are required to calculate the totient and private key.  Normally, this would be impossible, but since the modulus is rather small for RSA, it can be done.  After a bit of browsing, I found factordb.com, which can find the prime factors of a 100 digit number in a fraction of a second.    This seems useful for our purposes.  We enter the modulus into the website, and it factors perfectly into two distinct primes!

The primes are 

    783420406144696097385833069281677113 and 783756020423148789078921701951691559.
    
Now, we need to calculate the totient, which is 
    
    783420406144696097385833069281677112 * 783756020423148789078921701951691558 
    
Once we have the totient, we need to calculate the private key by finding the modular multiplicative inverse of the public exponent.  Thankfully, modular multiplication is commutative and associative, so even if Zob supplied us with the wrong public exponent, we can simply plug in the totient and private key and calculate the correct one.  The python code below does the job well.

    def egcd(a, b):
        if a == 0:
            return (b, 0, 1)
        else:
            g, y, x = egcd(b % a, a)
            return (g, x - (b // a) * y, y)
    
    def modinv(a, m):
        g, x, y = egcd(a, m)
        if g != 1:
            raise Exception('modular inverse does not exist')
        else:
            return x % m
    print(modinv(340035333160956238336074318075946961695270890880263371398510321485728225,614010459838253953596498114943057696275499211319221374644408743572220496))
    >>> 65537

The program outputs 65537 as the modular inverse.  Normally, that is the most common RSA <i><b>Public</b></i> exponent (hmm... it seems like Zob really didn't know what he was doing.)  This indicates that the private exponent was actually provided to us by Zob - how wonderful!

Now we have all the components (c,d,n) required to decrypt the message.  The RSA decryption process is given by m = c^d mod n.  We do just that with the pow() function in python, converting the final result to hex to facilitate decryption.

    >>> c = 416808431213057812839807235099929401146034654633863359116938353620975451
    >>> d = 340035333160956238336074318075946961695270890880263371398510321485728225
    >>> n = 614010459838253953596498114943057697842675637887066261109163514805589167
    >>> print(hex(pow(c,d,n))[2:])
    >>> 666c61677b6c306c5f6c336c5f6b316b5f6b336b5f3b703b7d
    
We now convert the flag to ASCII text, which is the flag.

#### Flag

flag{l0l_l3l_k1k_k3k_;p;}
