---
title:  "Picograd"
date:   2023-02-23 16:54:21 +0100
#permalink: /hello-world/
#feature-img: /assets/images/hello-world.jpg

#header:
#  image: /assets/hello-world.jpg
---

As part of my master thesis I implemented a [spiking neural network](https://en.wikipedia.org/wiki/Spiking_neural_network) from scratch on a microcontroller. 
Because trainig was done seperately on a PC, only the forward pass was needed. Which was a big relief, because PyTorches autograd always seemed like black magic to me (even after learning the theoretical background).
So seeing a a post on Hackernews about [Andrej Karpathys](https://en.wikipedia.org/wiki/Andrej_Karpathy) amazing video series on building neural networks from scratch, seemed like the perfect opportunity to fix this blind spot of mine. That means this project will probably be **heavily**  inspired by his [micrograd](https://github.com/karpathy/micrograd).

## Goal
The goal is to create the whole machine learning pipeline from scratch which will successfully be able to learn a task.

- Implement a neural network pipeline from scratch (forward and backward pass, [SGD](https://en.wikipedia.org/wiki/Stochastic_gradient_descent))
- Don't use any machine learning libraries (including NumPy)
- Get decent accuracy on [MNIST](https://en.wikipedia.org/wiki/MNIST_database), let's say 0.8

## Documentation

### Watching the lecture
Over the course of a couple of weeks I watched the first [video](https://www.youtube.com/watch?v=VMj-3S1tku0) in Andrej Karpathys [Neural Networks: Zero to Hero
](https://karpathy.ai/zero-to-hero.html) series. With the help of a Jupyter notebook he implements a version of micrograd that implements everything from backpropagation to the ability to learn a simple binary classification task with a multilayer perceptron. In my opinion the explanation is done **exeptionally** well, relying mostly on code to explain the concepts (as opposed on equations like many university courses would).  
Sometimes that feeling of understanding can be deceitful: Even when something is well understood in theory, actually using it in an excercise or even the real world can often reveal some blind spots or unforseen difficulties.
So as an **extra challenge** I decided to do my whole implementation without referencing his video or the micrograd code. 

### Computational graph and backpropagation
So let's finally start writing code! I also use a Jupyter notebook for this, as it is very convenient for quick prototyping.  
The simple part is implementing the forward pass that builds a [directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) which remembers the order of all computations. Every operation takes a number of input vertices (each representing a number) and produces one output vertex (the result of the operation). Addition would look something lik: a(child) + b(child) = c(parent)
At the heart of backpropagation is the backward() function that propagates the gradient of the parent to its children. For this, the partial derivate of every function in the forward pass has to be written out.  
For me the interesting part was calling the backward function for every vertex in the graph: Initially I thought this could be done recursively from the output (like [DFS](https://en.wikipedia.org/wiki/Depth-first_search)), but this doesn't work because the graph isn't strictly a tree and the gradient of each vertex has to be fully calculated before propagating it to the children. **TODO: Image?** So the updates have to be done in topological order which can be computed by using [topological sorting](https://en.wikipedia.org/wiki/Topological_sorting). This was where I was _very_ tempted to look at the micrograd implementation of toposort, because Andrej has a very elegant implementation that does it in only a couple of lines of code. But with a bit of help from ChatGPT I came up with my own version that looks like this:

{% highlight python %}
visited = set()
topo_order = []
stack = [self]

while stack:
    curr = stack.pop()
    if curr not in visited:
        visited.add(curr)
        stack.extend(curr.children)
        topo_order.append(curr)
{% endhighlight %}          

As opposed to the one in micrograd, this sorts the graph directly into the correct order for backprop and does not have to be reversed first.  
With all this complete, the output gradient can be propagated throughout the whole graph.

### Implementing the neural network
- neuron
- linear layer
- loss function