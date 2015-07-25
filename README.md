# mersenne-twister-recover
Given at least 624 outputs of a Mersenne Twister we can restore its internal state.
The Mersenne Twister is used as PRNG in many programming languages such as PHP, Python, Ruby, etc. 
Please refer to [Mersenne Twister](https://en.wikipedia.org/wiki/Mersenne_Twister) for detailed descriptions.



## Usage
Simply pass a list of observed outputs to the go() method:

```
  r1 = random.Random(31337)
  outputs = [r1.getrandbits(32) for _ in range(625)]
  
  mtr = MT19937Recover()
  r2 = mtr.go(outputs)
  
  assert r1.getrandbits(32) == r2.getrandbits(32)
```

### Internal State
The internal state of a Mersenne Twister consists of 624 integers and an index.
Whenever a random number is requested, a tempering function is applied to the integer to which the index is currently pointing and the index is incremented.
When the index reaches 624, a new set of 624 integers is generated, which is called the *twist*.

### Recovering internal state
Recovering the internal state is done by applying the inverse of the tampering function to each observed output, which will yield the internal 624 integers. 
However, if the observed 624 integers were not generated directly after a twist, another observed output is required to recover the correct index.
