---
layout: post
title: "Game Engine Scripting - Semester 1 Project"
categories:
  - Projects
  - index
tags:
  - Game Engine Scripting
  - Projects
  - Tutorial
---

For my assignment in semester one I needed to choose a topic from one of my lectures to create a video tutorial that could be used to teach someone about a chosen subject. I chose to create a video tutorial on L-systems, the process of procedurally creating trees and other assets through predefined rules. Additionally, I created an accompanying UI so the user could input their own rules and see how the trees change. You can follow along with the below tutorial video or with the written guide below.

<iframe width="1200" height="800" src="https://www.youtube.com/embed/AWDEfN0eeWI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

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

This script will create a Vector3 to store the position of a line and a quaternion to store the rotations of the lines.

Once the script has been completed you will want to navigate back to the scripts folder and create a new script called 'LSystemsGeneration.cs'. Go ahead and open this file, first of all we are going to add System, this will help us to use System.text, next we will add System.text, this allows us to edit unity text fields such as the ones used to display text in UI, finally we are going to add UnityEngine.UI, this allows us to edit unity UI from within scripts.

The next step is to start creating some variables, first of all we are going to create our number variables, we are going to need an angle float variable for which controls the angles of the branches, then a length float variable to control how many units forward the trees are going to go, next a width float variable for how wide we want our trees to be, now an iterations integer variable needs to be create to define how big we want our tree to be, an integer has been used instead of a float because we don't want half a iteration, finally a fieldofView float is made to define the starting FOV of the camera.

Now we need to create our axiom, this can just be a simple string variable and the same for the currentString variable but we want to initialise this variable to be an empty string.

Next we need to create a dictionary that accepts a char as its key and a string as its value. A stack also needs to be used so we can keep track of the position and rotation of our branches and it does this using the position and rotation variables made in the transforminfo.cs script.

Four game objects need to be created, one called treeParent, one called branch, one called treeSegment and one called Tree and we want the Tree game object to be initialised to null.

```cs
using System.Collections;
using System.Collections.Generic;
using System.Text;
using UnityEngine;
using System;
using UnityEngine.UI;

public class LSystemGeneration : MonoBehaviour
{
    /// <summary>
    /// Setting up some varialbes that will be used to controll the size of our trees, angle and the iterations
    /// </summary>
    private float angle;
    private float length;
    private float width;
    private int iterations;
    public float fieldOfView;

    private string axiom; // this is the starting state for the generation and will not be changed unless a new pattern is required
    private string currentString = string.Empty; // This will be used to store the current pattern

    [SerializeField] private Dictionary<char, string> rules; // creates a dictionary that takes a char as its key and a string as the value
    private Stack<TransformInfo> transformStack; // by using the transforminfo script we can create a stack using the info in the script

    [SerializeField] private GameObject treeParent; // This groups together our branches under a single parent
    [SerializeField] private GameObject branch; // This is what will make up our trees
    private GameObject Tree = null; // creates an gameobject which is empty
    private GameObject treeSegment; // this will be used to give our individual branches settings based on the pattern
```

Next is to create our input fields that will be used to update the sizes of our trees, the dictionary and the actual buttons that will be used to generate the tree. We also define two colours which will be used to color our trees, however on the 'endcolour' is used. Finally we create strings that will be used to store are axiom and the rules for the dictionary.


```cs
  /// <summary>
  /// This is a group of UI elements that control the scene when in play mode
  /// </summary>
  [SerializeField] private InputField inputFieldAngle;
  [SerializeField] private InputField inputFieldHeight;
  [SerializeField] private InputField inputFieldWidth;
  [SerializeField] private InputField inputFieldIterations;
  [SerializeField] private InputField inputFieldKey;
  [SerializeField] private InputField inputFieldValue;
  [SerializeField] private Button generateButton;
  [SerializeField] private Button resetButton;
  [SerializeField] private Button addtodictionary;
  [SerializeField] private Slider FOVSlider;

  private Color startColour = new Color(139, 69, 19, 0);
  private Color endColour = Color.green;

  private string inputFieldAxiom;
  private string inputFieldRules;
```
