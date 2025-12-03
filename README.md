# TNTools
An old collection of tools for solving classical combinatorial optimization problems with Tensor Networks. This lib was developped as I was doing my master thesis. 

The idea is to approximate the function values of all assignation through a MPS (Matrix Product State). This is done mainly by applying operators for weigths and constraints. An adapted DMRG procedure can then be applied to find the coordinates of the largest component value of the MPS. These coordinates are directly equivalent to the equivalent combinatorial optimization problem's answer.

Explained in chapter 3 of my [thesis](https://usherbrooke.scholaris.ca/bitstreams/d8f0bc84-b9b5-4f92-a9dd-bd35ecb11836/download).


**This lib is beginning to date and shows how my code came to improve over the years.**


### Why not simply use other TN libraries?

Regular TN libraries are optimized and built with either simulation of quantum systems or machine learning in mind. Because of that, I would have needed to overwrite a lot of functionnalities in these libs and would have dealt with a much heavier codebase. I wanted this library to be exactly tailored to what it is attempting to solve: Combinatorial Optimization. 

Developping this library was also a great way to develop coding skills and be much more aware of the nooks and crannies that go into these simulations. Obviously, that code is inefficient and would require serious refreshing for it to ever be widely used in the community. Forking or building on top of an already popular TN library would obviously be best practice in such a scenario. 

### Identified (would be) immediate needs for refactor

- Simplification and restructure
- Split in clean submodules
- Add proper docstring
- Implement JIT compilation
- Clean Syntax and naming


### Example snippet

```{python}
import numpy as np

import tntools as tnt


# Some bit string
bit_entry = np.array([0,0,1,0,0])

# encoded into an MPS
entry_mps = tnt.binary_mps(bit_entry)
# Force MPS in canonical format
state_mps = tnt.MpsStateCanon(entry_mps)
state_mps.create_orth()

# Noise added through Boltzmann weigth MPO
id_mpo = tnt.boltz_mpo(len(bit_entry), b_prob=0.1)
state_mps.mpo_contract(id_mpo)


# We can use the dephased DMRG method to get back the main component's position
main_comp = state_mps.main_component("dephased_DMRG")

# Which is the original bitstring
print(main_comp)
```


