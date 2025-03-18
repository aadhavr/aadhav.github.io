<!-- ---
title: "Studying the State of AI in a Day"
layout: post
date: 2024-09-12 13:29
headerImage: false
tag:
- ai
- learning
category: blog
description: Markdown summary with different options
--- -->

Last week, I moved down to UCSB, where I promptly sprained my ankle after a party. I find myself now confined to my bed. Given OpenAI's o1 announcement, I wanted to understand the major papers in AI over the past decade. 

This post is intended to help myself and other understand the infrastructure that is built upon with each better model.

## Papers to Study

This list, and a lot of my understanding of the landscape comes from [this video](https://youtube.com/watch?v=am7bJc5xC8s&t=475s) by Andrew Trask (@[iamtrask](https://x.com/iamtrask)). It's been a huge resource, and I'd recommend checking it out.

- [**Large-sale Unsupervised Learning Using Graphics Processors**](https://robotics.stanford.edu/~ang/papers/icml09-LargeScaleUnsupervisedDeepLearningGPU.pdf), Andrew Ng et al., 2009.
- [**ImageNet Large Scale Visual Recognition Challenge 2010**](https://www.image-net.org/challenges/LSVRC/2010/index.php#cite), Jia Deng et al., 2010.
- [**ImageNet Classification with Deep Convolutional Neural Networks**](https://proceedings.neurips.cc/paper_files/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf), Alex Krizhevsky et al., 2012.
- [**Distributed Representations of Words and Phrases and their Compositionality**](https://arxiv.org/pdf/1310.4546), Tomas Mikolov et al., 2013.
- [**Playing Atari with Deep Reinforcement Learning**](https://arxiv.org/abs/1312.5602), Diederik P. Kingma et al., 2013.
- [**Mastering the game of Go with deep neural networks and tree search**](https://www.nature.com/articles/nature16961), Volodymyr Mnih et al., 2015.
- [**Attention is All You Need**](https://arxiv.org/abs/1706.03762), Ashish Vaswani et al., 2017.
- [**Improving Language Understanding by Generative Pre-Training**](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf), Alec Radford et al., 2018.
- [**Language Models are Unsupervised Multitask Learners**](https://hayate-lab.com/wp-content/uploads/2023/05/61b1321d512410607235e9a7457a715c.pdf), Alec Radford et al., 2019.
- [**Language Models are Few-Shot Learners**](https://arxiv.org/pdf/2005.14165), Tom B. Brown et al., 2020.
- [**Training language models to follow instructions with human feedback**](https://arxiv.org/pdf/2203.02155), Long Ouyang et al., 2022.


## Things to Rebuild

- [AlexNet](https://blog.paperspace.com/alexnet-pytorch/)
- [Word2Vec](https://jaketae.github.io/study/word2vec/)
- [Variational Autoencoder](https://medium.com/@sofeikov/implementing-variational-autoencoders-from-scratch-533782d8eb95)
- [Deep Q-Networks](https://www.tensorflow.org/agents/tutorials/0_intro_rl)
- [Tranformer Model](https://towardsdatascience.com/build-your-own-transformer-from-scratch-using-pytorch-84c850470dcb)
- [GPT-2](https://www.youtube.com/watch?v=l8pRSuU81PU&t=119s)

###  Large-sale Unsupervised Learning Using Graphics Processors

This paper looks at how to harness vast amounts of data when you lack the labels that normally guide learning algorithms. Imagine you’re teaching a machine to recognize objects in pictures, but you’ve only shown it raw pixels—no instructions on what’s a cat, a car, or a cloud. 

The authors focus on two methods, deep belief networks (DBNs) and sparse coding, which are powerful unsupervised learning techniques. These methods are good at finding hidden patterns in complex data. But training these models takes a long time on standard computers. We’re talking weeks to train large models with millions of parameters. That's where modern graphics processing units (GPUs) come into play.

GPUs, originally designed for video games, can process many tasks in parallel, much faster than the central processing units (CPUs) that run most computers. This paper shows how to adapt DBNs and sparse coding to GPUs, making it possible to train these models in hours rather than weeks.

In DBNs, layers of artificial neurons learn to represent data at increasing levels of abstraction. For example, a lower layer might capture basic features like edges in an image, while higher layers detect complex patterns like faces. Training such a model requires repeatedly adjusting the network's parameters, but the GPU massively speeds up these adjustments by performing many computations simultaneously.

Sparse coding, on the other hand, works by breaking down data into a combination of simple building blocks. Think of it as trying to express a complex image using just a handful of brushstrokes. The fewer strokes you need, the “sparser” your representation. Again, the authors found a way to adapt this technique for GPUs, achieving a significant speed boost.

The results are impressive: the GPU-based approach is up to 70 times faster than traditional methods for DBNs and 15 times faster for sparse coding. For researchers, this means they can train much larger models in a fraction of the time.

### Imagenet CNN

Imagine a computer trying to tell apart cats from dogs by showing it millions of pictures, without telling it what to look for. Alex Krizhevsky, Ilya Sutskever, and Geoffrey Hinton, in their groundbreaking 2012 paper, took a massive collection of images, known as *ImageNet*, which has over 1.2 million images split into 1000 different categories, and trained a neural network to recognize objects. Specifically *Convolutional Neural Networks (CNNs)*, a type of model that’s inspired by how our own brains process visual information.

Let’s break it down. A CNN works by processing an image in layers, each layer learning a little more complex than the last. These layers have filters that slide, or convolve, an image across layers. These filters are small grids, say 3x3, that take the values assigned to a 3x3 grid of pixels in the image, and then calculate the dot product. When a filter passes over an image, it multiplies its weights with the corresponding pixel values from the image and sums the results to produce a feature. This can help us find edges, corners, or other patterns. Early layers capture simpler pattern, while deeper layers learn complex patterns, like the shape of a nose or the texture of fur. The output of a convolution is called a feature map, which shows where a specific, meaningful pattern appears in the image.

After convolution, the output passes through a non-linear activation function like ReLU, Rectified Linear Unit. What this does is makes the negative values in the feature map equal to 0, because real-world data is rarely linear. CNNs also use a technique called pooling. Pooling layers simplify feature maps to reduce their size, making the network faster and less prone to overfitting. In other words, it preserves the important information and discards less critical details.

<figure>
  <img src="/assets/images/relu-fig1.png" alt="Alt text">
  <figcaption style="text-align: center;"><i>Figure 1: A four-layer convolutional neural network with ReLUs (solid line) reaches a 25% training error rate on CIFAR-10 six times faster than an equivalent network with tanh neurons (dashed line). The learning rates for each network were chosen independently to make training as fast as possible. No regularization of any kind was employed. The magnitude of the effect demonstrated here varies with network architecture, but networks with ReLUs consistently learn several times faster than equivalents with saturating neurons.</i></figcaption>
</figure>


Now there are two kinds of layers in a CNN: convolutional layers focus on salient features and fully connected layers take in every single input and connect it to every output neuron. Once the image is reduced to a high-level feature representation, the fully connected layers try to make a prediction. For instance, a fully connected layer might take all the information about edges, textures, and shapes to decide, “Yes, this is a cat.” 

CNNs learn to adjust the weights of their filters and neurons. The network will predict "cat" and a loss function will calculate how wrong this prediction is. Then, using backpropagation, the network calculates how each weight contributed to the error. The weights are updated with an optimizer like stochastic gradient descent or ADAM, and the process repeats agains for the next image. This is how a CNN learns which patterns to detect are useful for making correct predictions. 

What makes this network special, aside from its sheer size, is how the researchers used GPUs to speed up the training process. Without GPUs, training a network this big would have taken weeks, maybe months, but instead took a couple of days. 

Now, training such a large model on so much data comes with a risk: overfitting. This is when the model gets too good at recognizing the training images but fails to generalize to new, unseen images. To combat this, the researchers used a clever trick called *dropout*. Dropout forces the network to forget certain connections between neurons at random during training. It’s like making a basketball player practice with one arm tied behind their back, so they’re forced to become more well-rounded. This technique helped the model avoid overfitting, and allowed it to perform better when presented with new images.

So, how well did this CNN perform? Exceptionally. On the test set from ImageNet, their model achieved a top-1 error rate of 37.5% and a top-5 error rate of 17.0%. What does that mean? The top-1 error rate is how often the most confident prediction is wrong and the top-5 error rate is how often the top 5 most confident predictions are all wrong. This paper set a new standard for image classification and solidified deep learning as the go-to approach for many computer vision tasks.

### Distributed Representations

This paper aims to improve distributed representations of words and phrases using the Skip-gram model, a neural architecture that learns vector embeddings for words by maximizing their ability to predict surrounding words in a given context.

The Skip-gram model predicts surrounding words for a given target word. The training objective is to maximize the probability of correctly predicting these surrounding words, which boils down to finding embeddings that position related words close together in a vector space. These embeddings can even solve analogies mathematically, like finding that the vector for "Paris" is close to the vector of "Spain" subtracted by the vector of "Madrid" added by the vector of "France." As you can imagine, the computations for millions of words are impractical so the researched introduced two clever approximations: hierarchical softmax and negative sampling.

