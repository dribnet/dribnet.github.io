---
layout: single
title:  "Perception Engines"
header:
  teaser: "/images/500x300.png"
categories: 
  - Art
tags:
  - neural networks
  - perception
date: 2018-03-31
permalink: /posts/2018/03/perception-engines/
---
A visual overview of the motivation and goals in developing a
a technique I call "perception engines". Perception engines
explore the ability of deep neural networks to create
recognizable abstract objects from collections of real world objects.
This system was used to
create "Treachery of Imagenet" - a series of 12 ink prints based
on ImageNet categories, and permutations of this system are the basis
of ongoing and future work.

**Post Status: Early Draft**

![Treachery of ImageNet: forklift, ruler, sewing machine](https://user-images.githubusercontent.com/945979/37252510-d35ac436-2586-11e8-85e8-f5247fa78a2a.jpg)
<p align="center">Three prints from the recent Treachery of ImagNet series</p>

(suggestions? please [suggest an edit](https://github.com/dribnet/dribnet.github.io/edit/master/_posts/2018-03-11-perception-ingines.md) as a merge request!)

Introduction
======
The core question I set out to explore: can modern neural networks
be used to create abstract forms from nothing other than collections
of examples segregated into an ontolgoy of human concepts? To preview
the result - a raw collection of concrete objects such as an image library
of hundreds of images electric fans:

![ImageNet: First 49 examples of Electric Fan](https://user-images.githubusercontent.com/945979/37562896-1154e040-2ad8-11e8-924a-50962da6d6e2.png)
<p align="center">4% of the "Electric Fan" category, randomly selected</p>

was transformed into an abstract visual representation for the collection
 - this ink two color ink print:

![Abstract represenation of fan: two color ink print](https://user-images.githubusercontent.com/945979/37562897-12e5bd9e-2ad8-11e8-8c59-38cc8e3f4387.png)
<p align="center">Abstract two color ink print representing the category of electric fans</p>



Themes
======

Harold Cohen and AARON. The role of perception in creativity. Creating
a system that highlights the active role of perception in creative work.
Goal is to create a system that can express abstract concepts within
the constraints of a drawing system. Here an abstract concept is grounded
completely by a dataset. Along the way we also explore the theme of how
datasets define ML concepts, including the arbitrary nature of the ImageNet
ontology and possible bias that surfaces.

Of note: interested in creating real, physical artifacts (like AARON)

Architecture
======

Repurposing the techniques of Adversarial Examples:

 * instead of constraints on making small changes to existing images, allow arbitrary changes within the constraints of a drawing system

 * interested in abstractions that generalize across neural networks (and humans), so target ensembles of trained networks

 * need to model the variations of physical production and presentation

Core
------

Three main modules:

  * Drawing system - The system of contraints involved in creating the physical artifact including anticipating a distribution variations. In practice this far these have been marks on a page under various production tolerances and lighting conditions.

  * Creative objective - What is the expressive goal? So far I have
  focusing on using neural networks pre-trained on ImageNet with an
  objective of maximizing response to a single ImageNet classe. This
  is also consistent with most Adversarial Example literature.

  * Planning system - How is the objective maximized? I use a blackbox
  search technique that does simple hill climbing. Though inefficient,
  it is otherwise a very simple technique and works well in practice
  over hundreds to thousands of iterations.

Combining the current versions of these systems is akin to building
a computation ouija board: several neural networks simultaneously
nudge and push a drawing toward a stated objective.

![First 78 steps in creating the Electric Fan drawing](https://user-images.githubusercontent.com/945979/37562776-4b316fa2-2ad5-11e8-857f-b1c29ba88318.gif)
<p align="center">First 78 steps in creating the Electric Fan drawing.</p>

At the end we
can test for generalization by querying neural networks that were not
involved if they agree the objective has been met - an analogue
of a train/test split where each entry is itself a trained network.

![Network responses to Electric Fan print](https://user-images.githubusercontent.com/945979/37253079-931239d0-2591-11e8-9533-a1904c900fce.png)
<p align="center">Electric fan was "trained" with input from inceptionv3, resnet50, vgg16 and vgg19 - and after printing scores well when evaluated on all four of those networks (circled in red). However, this result also generlizes well to other networks as seen by the strong top-1 scores on four other networks tested (and a lower top-3 score on nasnetmobile).</p>

This also inverts the traditional creative relationship employed in human computer interaction. Instead of using the computer as a tool, the Drawing System module can be thought of a special tool that the neural network itself drives to make creative outputs. Thus, one of my core creative inputs is the design of a programming design system that allows the neural network to express itself with expressivity and a distinct style.

Early examples of (non-physical) Drawing Systems
------
![Early images: birdhouse, traffic light, school bus](https://user-images.githubusercontent.com/945979/37252509-cfcf4ed6-2586-11e8-9730-519f37f5b855.png)
<p align="center">Early drawing systems used lines and rectangles with unrestrained colors.</p>

Modeling physical artifacts
------
![Left: Loading Purple Ink drum into Riso printer / Right: "Electric Fan" print before adding second black layer](https://user-images.githubusercontent.com/945979/37252784-67e24ac0-258c-11e8-8cab-565717e2284b.png)
<p align="center">Left: Loading Purple Ink drum into Riso printer<br>
Right: "Electric Fan" print before adding second black layer.</p>

After the proof of concept I was ready to target a physical drawing system. I chose a Riso Printer because
it is a physical ink process very much like silkscreening. This meant I would be subject to a limited number of
ink colors I could get (in practice, about 6) as well as unpredictable layer alignment between layers of
different colors.

At this point I was also provided a grant from Google's Artist and Machine Intelligence group (AMI).
With their generous support, I was able to print a series of test prints and iteratively improve
my software system to model the physical printing process. Each of these is modeled as a distribution
of possible outcomes.

Issue #1: Layer Alignment
-----------
It is common for Riso prints to have a small amount of mis-alignment between layers because the paper must
be run through for each different color. This possibility was handled by apply a small amout of jitter
manually between colors.

![Electric fan with jitter](https://user-images.githubusercontent.com/945979/37575710-7b18fe0c-2b8d-11e8-8db2-a1490a831dfd.gif)
<p align="center">Examples of jitter being applied to produce a distribution of possible alignment outcomes.</p>

Issue #2: Lighting
-----------
The colors of a digital image can be given exactly. But a physical item will be perceived with slightly different
colors depeding on the ambient lighting conditions. To allow the final print to be effective in a variety of
environments, the paper and ink colors were photographed under multiple conditions and then simulated with
various possibilities. 

![Electric fan with different lighting conditions](https://user-images.githubusercontent.com/945979/37575709-7aefb862-2b8d-11e8-88e9-9e78e24b6b96.gif)
<p align="center">Examples of lighting variations being applied to produce a distribution of possible alignment outcomes.</p>

The lighting and layer adjustments were independent and could be applied concurrently.

![Electric fan: jitter and lighting](https://user-images.githubusercontent.com/945979/37575708-7ac59c62-2b8d-11e8-9b9c-a65024631b26.gif)
<p align="center">Examples of jitter and lighting variations being applied to produce a larger distribution of possible alignment outcomes.</p>

Issue #3: Perspective
-----------
In the physical setting, the exact location of the viewer is not known. To keep the print from being dependent
on a particular viewing angle, a number of perspective transformations were also applied. These are generally
added during a final refinement stage and are done in addition to the alignment and lighting adjustments.

![Electric fan with perspective transformations](https://user-images.githubusercontent.com/945979/37575707-7a9a26ae-2b8d-11e8-873c-4ace927313f0.gif)
<p align="center">Examples of perspective transform being added to produce a distribution of possible viewing angles.</p>

Final Output
-----------
Once the system has produced a candidate, a set of master prints are made. Importantly, the perspective
transforom is disabled to produce the master print in its cononical form. For the fan print, two layers
were produced: one for the purple ink and one for black.

![Separated layers](https://user-images.githubusercontent.com/945979/37576303-afdd6544-2b90-11e8-9748-5ef7e8d71747.png)
<p align="center">Final aligned layers of Electric Fan as they are sent to be printed.</p>

These are then used to print a series of ink versions on paper.

![Example print](https://user-images.githubusercontent.com/945979/37562897-12e5bd9e-2ad8-11e8-8c59-38cc8e3f4387.png)
<p align="center">One of the ink prints from the master above (no two are exactly alike).</p>


Treachery of Imagenet
=====================
![Treachery of ImageNet: complete series](https://user-images.githubusercontent.com/945979/37252592-3c064fa4-2588-11e8-947a-0f7c89fe50fa.jpg)
<p align="center">All 12 prints in the Treachery of ImageNet series</p>


Ongoing work
============

Currently two other related series are in various stages of production. These use the same objective and planner, but vary the drawing system.

 * a series again using the Riso printer, but modeling multiple ink layers

 * a series using a more generic screen printing technique

Further out, other series are being prototyped which using different objectives or more radical departures from the current types of drawing systems.