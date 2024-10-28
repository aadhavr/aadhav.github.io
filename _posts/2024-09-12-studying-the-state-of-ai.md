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

This paper dives into a fundamental challenge in machine learning: how to harness vast amounts of data when you lack the labels that normally guide learning algorithms. Imagine you’re teaching a machine to recognize objects in pictures, but you’ve only shown it raw pixels—no instructions on what’s a cat, a car, or a cloud. That’s the realm of unsupervised learning—it’s like trying to solve a puzzle without knowing the final picture.

The authors focus on two methods—deep belief networks (DBNs) and sparse coding—which are powerful unsupervised learning techniques. These methods excel at uncovering hidden patterns in complex data. However, there’s a catch: training these models typically takes an excruciatingly long time on standard computers. We’re talking weeks to train large models with millions of parameters. That's where modern graphics processing units (GPUs) come into play.

GPUs, originally designed for video games, can process many tasks in parallel, much faster than the central processing units (CPUs) that run most computers. This paper shows how to adapt DBNs and sparse coding to GPUs, making it possible to train these models in hours rather than weeks.

In DBNs, layers of artificial neurons learn to represent data at increasing levels of abstraction. For example, a lower layer might capture basic features like edges in an image, while higher layers detect complex patterns like faces. Training such a model requires repeatedly adjusting the network's parameters, but the GPU massively speeds up these adjustments by performing many computations simultaneously.

Sparse coding, on the other hand, works by breaking down data into a combination of simple building blocks. Think of it as trying to express a complex image using just a handful of brushstrokes. The fewer strokes you need, the “sparser” your representation. Again, the authors found a way to adapt this technique for GPUs, achieving a significant speed boost.

The results are impressive: the GPU-based approach is up to 70 times faster than traditional methods for DBNs and 15 times faster for sparse coding. For researchers, this means they can train much larger models in a fraction of the time, unlocking new possibilities for applications like image recognition and natural language processing.

By leveraging the raw computational power of GPUs, this paper not only accelerates existing algorithms but also opens the door to even more complex machine learning models. This is a major step forward for the field, where speed and scale are often the limiting factors. In essence, it’s like upgrading from a bicycle to a racecar—you’re still on the same road, but now you can go a lot further, a lot faster.

### Imagenet CNN

Imagine you’re tasked with teaching a computer how to tell a cat from a dog just by showing it millions of pictures—without telling it what to look for. That’s pretty much what Alex Krizhevsky, Ilya Sutskever, and Geoffrey Hinton did in their groundbreaking 2012 paper. They took a massive collection of images, known as *ImageNet*, which has over 1.2 million images split into 1000 different categories, and trained a neural network to recognize objects. But here’s the kicker: they did it using deep learning, specifically *Convolutional Neural Networks (CNNs)*—a type of model that’s inspired by how our own brains process visual information.

Let’s break it down. A CNN works by processing an image in layers, each layer learning something a little more complex than the last. The first layer might detect simple things like edges, while the last one figures out if it’s looking at a picture of a tiger or a toaster. The CNN in this paper is huge—it has 60 million parameters and 650,000 neurons, organized into five convolutional layers and three fully connected layers. These layers are like a funnel: they take a gigantic amount of input data (all those pixels!) and condense it down into predictions—essentially saying, “Hey, this looks like a car with 90% certainty.”

What makes this network special, aside from its sheer size, is how the researchers used GPUs—graphic cards typically used for video games—to dramatically speed up the training process. Without GPUs, training a network this big would have taken weeks, maybe months. By leveraging the power of GPUs, they cut the training time to just five or six days. That’s like building a jet engine and then strapping it to a skateboard—it makes the whole thing fly.

Now, training such a large model on so much data comes with a risk: overfitting. This is when the model gets too good at recognizing the training images but fails to generalize to new, unseen images. To combat this, the researchers used a clever trick called *dropout*. Dropout forces the network to forget certain connections between neurons at random during training. It’s like making a soccer player practice with one arm tied behind their back, so they’re forced to become more well-rounded. This technique helped the model avoid overfitting, and allowed it to perform better when presented with new images.

So, how well did this CNN perform? Exceptionally. On the test set from ImageNet, their model achieved a top-1 error rate of 37.5% and a top-5 error rate of 17.0%. What does that mean? Well, in the top-5 error test, the network tries to guess the object in an image, and it’s allowed five guesses. Their network was right about 83% of the time, significantly beating out other models that topped out at around 74%.

This paper set a new standard for image classification and solidified deep learning as the go-to approach for many computer vision tasks. By building a massive CNN, optimizing it for GPUs, and using dropout to prevent overfitting, they paved the way for future breakthroughs in everything from facial recognition to self-driving cars. In short, they didn’t just teach a machine to recognize cats—they revolutionized how machines see the world.

### Distributed Representations

