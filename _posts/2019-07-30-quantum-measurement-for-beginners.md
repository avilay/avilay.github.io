---
layout: post
title: Quantum Measurement for Beginners
date: '2019-07-30 07:00:00'
tags:
- from-medium
---

As I embarked on my journey to learn quantum computing, one thing that thoroughly confused me was the concept of quantum measurement. I wanted to share my understanding here in the hopes that it’ll help other fellow travelers on the same journey! (Can’t wait to get to the math right away? Check out my notes [here](https://avilay.gitlab.io/gyan/quantum-computing/measurements.html)).

Lets start with the archetypal single qubit quantum state -

![fig2-1](/assets/imgs/quantum-measurement-for-beginners/fig2-1.png)

By now you must have heard of the Born Rule that says that the probability of measuring this state as |0⟩ is |α|² and that of measuring it as |1⟩ is |β|². But what does “measuring” a quantum state even mean? And why do we say measuring it _as state_ |0⟩ or |1⟩? We even hear about the “expected value of an operator”, where does that come from? The rest of this post attempts to clarify these questions.

One of the first things we need to understand is that there is no standard “meter” that we can use to measure a quantum state. It has to be measured using some “observable” which is a quantum operator, which in turn can be represented as a matrix. Physically we can think of this observable aka matrix as some function of the Hamiltonian of the quantum system with discrete ground and excited states. But I really don’t c̶o̶m̶p̶r̶e̶h̶e̶n̶d̶ care about the Physics of this stuff. Lets stick to the Math. Let _H_ be such a matrix for our single qubit state. **_The special thing about this matrix is that its eigenvectors are our basis states |0⟩ and |1⟩_**. Let their corresponding eigenvalues be some quantities ε0 and ε1.

![fig3-1](/assets/imgs/quantum-measurement-for-beginners/fig3-1.png)

According to the **_measurement postulate_** , which is one of the four fundamental postulates of quantum computing, when we use _H_ to measure our state |ψ⟩, we will get a reading of either ε0 or ε1 with the following probabilities:

![fig4-1](/assets/imgs/quantum-measurement-for-beginners/fig4-1.png)

Where,

![fig5-1](/assets/imgs/quantum-measurement-for-beginners/fig5-1.png)

After some [algebraic manipulations](https://avilay.gitlab.io/gyan/quantum-computing/measurements.html) we can show that -

![fig6-1](/assets/imgs/quantum-measurement-for-beginners/fig6-1.png)

This explains why we see the terms |α|² and |β|². We can even see that the expected value of _H_ when measuring |ψ⟩ is -

![fig7-1](/assets/imgs/quantum-measurement-for-beginners/fig7-1.png)

but it still doesn’t explain why we say “measuring the state as |0⟩ or |1⟩”. Once again the measurement postulate comes to our rescue. It says that _if we get a reading of ε0_then |ψ⟩ will transition to some other state |ψ’⟩ given by -

![fig8-1](/assets/imgs/quantum-measurement-for-beginners/fig8-1.png)

After some [more algebraic manipulations](https://avilay.gitlab.io/gyan/quantum-computing/measurements.html) we can show that -

![fig10-1](/assets/imgs/quantum-measurement-for-beginners/fig10-1.png)

And now if use _H_ to measure |ψ’⟩, we will always get a reading of ε0 and the resulting state does not change from |ψ’⟩, which is why we say that the state has “collapsed” to this new state.

One thing that is not immediately obvious is that if take a completely different observable with different eigenvalues/eigenvectors, and use that to measure the same state |ψ⟩, we will get completely different readings and resulting states. Generally when we want to measure a state in the standard basis, i.e., |0⟩ and |1⟩, we use Pauli’s Z operator because its eigenvectors are |0⟩ and |1⟩. And when we want to measure the same state in the Hadamard basis, i.e., |+⟩ and |-⟩, we use Pauli’s X operator, again because its eigenvectors are |+⟩ and |-⟩.

* * *

To summarize, the measurement postulate tells us that -

- We can only measure a quantum state with an observable.
- Upon measurement, we will get one of the eigenvalues of the observable as our reading.
- After measurement, the state will collapse into the eigenvector of the eigenvalue that we measured.
- Different observables used on the same initial state will yield different readings and different resulting states depending on their eigenvalues/eigenvectors.

> Image by [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2434282) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2434282)

