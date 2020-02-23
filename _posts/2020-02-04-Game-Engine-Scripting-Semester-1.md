---
layout: post
title: "Game Engine Scripting - Semester 1 Project"
categories:
  - CT5009 - Game Engine Scripting
tags:
---

For my assignment in semester one I needed to choose a topic from one of my lectures to create a video tutorial that could be used to teach someone about a chosen subject. I chose to create a video tutorial on L-systems, the process of procedurally creating trees and other assets through predefined rules. Additionally, I created an accompanying UI so the user could input their own rules and see how the trees change. You can follow along with the below tutorial video or with the written guide below.

[![](http://img.youtube.com/vi/HAgKdvAcR5g/0.jpg)](http://www.youtube.com/watch?v=HAgKdvAcR5g "")

## History of L-Systems
First of all, lets talk about what L-Systems are and how the work. The L-System was create by Hungarian Aristid Lindenmayer and they were originally known as Lindemayer Systems but the name was shortened to L-Systems, they were originally created to describe the behaviour and growth patterns of plant cells but we can also apply these techniques to create our own plants and trees within video games.

## How L-Systems Work
L-Systems are a collection of rules about what to do next, we use a collection of symbols which will affect how the tree grows, this collection of symbols is also known as a alphabet which contains constants, symbols which are not replaced after each iteration, and variables, symbols which can be replaced after each iteration. Next, there is a starting string which contains symbols from the alphabet which defines how the L-System starts off, this starting string can also be called an axiom. Finally, a set of rules are created which defines how variables from the alphabet should be replaced after each iteration. These three components can often be called a tuple as is represented with this equation 𝐺 = (𝑉, 𝜔, 𝑃) where "𝐺" is the tuple, "𝑉" is the alphabet, "𝜔" is the axiom and "𝑃" is the set of rules.

### The alphabet
A common alphabet is made up using a string of characters but this is not always the case, an alphabet could be made up using symbols or numbers but characters are used as they are more intuitive for the process. A simple alphabet may look like 'F', '+', '-' where 'F' means draw forward a set amount of units, '+' means turn right by 90 degrees and '-' means turn left by 90 degrees.

### The axiom
The axiom is the starting point of the L-System, the axiom is often very system and is derived from the alphabet. A simple axiom from our alphabet may be "F".

### The rules
The rules are normally replacement rules, e.g. F = F+F-F-F+F, this means after each iteration we replace "F" with the pattern listed, there the second iteration becomes F+F-F-F+F+F+F-F-F+F-F+F-F-F+F-F+F-F-F+F+F+F-F-F+F, this goes on for however long we want it to.

## The Implementation
First of all, you need to create a new unity project and call it 'L-Systems'. Once you have created a new project navigate to your project tab and create three new folders, one called Scripts, another called UI Assets and a final one called Prefabs. Now with these three folders created navigate to your scripts folder and create a new script called 'TransformInfo.cs'

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class TransformInfo : MonoBehaviour
{
    public Vector3 position;
    public Quaternion rotation;
}
```  

This script will create a Vecto3 to store the position of a line and a quaternion to store the rotations of the lines.
