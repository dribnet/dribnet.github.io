---
layout: single
title:  "Draft Post: Perception Engines"
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

This post gives an overview of the motivation and goals of explorations
covering the past 9 months. Using a technique called "perception engines",
I've been investigating the ability of deep neural networks to create
abstract imagery that is both recognizable. This system was used to
create "Treachery of Imagenet" - a series of 12 ink prints based
on ImageNet categories, and permutations of this system are the basis
of ongoing and future work.

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
nudge and push a drawing toward a stated objective. At the end we
can test for generalization by asking neural networks that were not
involved if they agree the objective has been met - an analogue
of a train/test split where each entry is itself a trained network.

This also inverts the traditional creative relationship employed in human computer interaction. Instead of using the computer as a tool, the Drawing System module can be thought of a special tool that the neural network itself drives to make creative outputs. Thus, one of my core creative inputs is the design of a programming design system that allows the neural network to express itself with expressivity and a distinct style.

Early examples of (non-physical) Drawing Systems
------

Modeling physical artifacts
------
Riso printer

 * colors and lighting
 * layer (mis)alignment
 * angle of view [Google Grant]

Treachery of Imagenet
=====================

Ongoing work
============

Currently two other related series are in various stages of production. These use the same objective and planner, but vary the drawing system.

 * a series again using the Riso printer, but modeling multiple ink layers

 * a series using a more generic screen printing technique

Further out, other series are being prototyped which using different objectives or more radical departures from the current types of drawing systems.