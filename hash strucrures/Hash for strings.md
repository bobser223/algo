

string S = "$c_{0}c_{1}...c_{n}$"

then hash = 
$$ (ord(c_0)*N^l + ord(c_1)*N^{l - 1} + ... +ord(c_{l-1})*N^1 + ord(C_l)) \% M $$



###  Implementation

```python 
const_prime = 31 # prime const num
M = 100007 # size of array

def H(S):
	h = 0
	for i in range(len(S)):
		h = h * const_prime + ord(S[i])
return h % M

```