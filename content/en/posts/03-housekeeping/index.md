---
title: "03: Refactoring"
summary: "Learn how to refactor your code"
categories: ["Post","Lesson"]
tags: ["main-course"]
date: 2025-02-25
draft: false
---

## Introduction

Refactoring is the process of improving your existing code without changing its external behavior. It’s like tidying up your workspace—everything stays the same functionally. This lesson will introduce some practical rules for deciding when to refactor, showcase refactoring examples in GDScript, and illustrate why you shouldn't worry about making everything perfect from day one.

## Growing State of the Project

As our project grows, so does the complexity of our codebase. Features get added, scripts get longer, variables multiply, and things can start to feel messy quickly. Periodic housekeeping—i.e., refactoring—keeps the project organized and maintainable. Although it may feel like an extra step, spending time on code improvements early on saves you from painful debugging sessions later.

## To Refactor or Not to Refactor

What looks “clean” to one person can look messy to another. Code style is somewhat subjective, and there's no single “correct” way to write code. However, there are a few scenarios where it’s almost universally accepted that refactoring is a good idea:

1. **Official Documentation Suggestion**: If you learn that the official engine or language documentation recommends a more efficient or idiomatic way to do something, it’s a sign you might need to adjust your code to align with standard practices.

2. **Repetition in Three Places**: The “Rule of Three” is a well-known guidline: if you find yourself duplicating the same code in three different places, it’s time to refactor and extract that logic into a single, reusable piece. This helps avoid inconsistencies and makes maintenance much easier down the road.

Beyond these points, don't stress too much about having picture-perfect code. What matters is your code works, and you understand how it works. You can always clean it up later.

## Refactoring Example

Throughout these lessons, you've seen some code that isn’t exactly “optimal.” This was intentional—to help you learn the basics step by step. Let’s explore a simple example related to variable initialization in Godot.

Previously, we used:
```gdscript
var animation_player

func _ready():
    animation_player = get_node("AnimationPlayer")
```

This approach is transparent: you create a variable, then assign it a node reference in the `_ready()` function. You clearly see when the value is set. However, a shorter (and generally cleaner) way to do the same thing is:

```gdscript
@onready var animation_player = $AnimationPlayer
```

Here’s what’s happening:
- `@onready` tells Godot to initialize this variable once the node is ready (i.e., after it enters the scene tree).
- `$AnimationPlayer` is shorthand for `get_node("AnimationPlayer")`.

This refactor reduces your code from two lines to one, and once you understand what’s going on, it’s faster to write and easier to read.

## Variable Export

Next, let’s talk about exporting variables to the Inspector. Sometimes you might find yourself repeatedly changing a variable's value directly in code (for example, the health of an enemy). Constantly editing script files can be tedious.

With Godot 4 (and newer), you can export a variable like so:

```gdscript
@export var hp = 3
```

This line exposes the variable in the Scene dock (the Inspector). You can then tweak it in the editor instead of digging into the script every time you want to adjust the value. This is especially helpful for quickly balancing game parameters like enemy health, damage, or speeds. Each instance of a scene can have a different value in the Inspector, while `3` remains the default if not modified.
<video src="instancing.mp4" controls></video>

Each instance of the scene can have a different value in the Inspector.
<video src="export_variable.mp4" controls></video>



## Adding a Speed Variable to the Character

Currently, your character’s movement speed is hardcoded. That makes it difficult to adjust the speed because you have to change the value in multiple places:

```gdscript
....

if Input.is_action_pressed("move_left"):
    direction.x = -200
if Input.is_action_pressed("move_right"):
    direction.x = 200
if Input.is_action_pressed("move_up"):
    direction.y = -200
if Input.is_action_pressed("move_down"):
    direction.y = 200

...
```

If you want to tweak the speed from `200` to `300`, you'd have to hunt down every instance. A better approach is to define a single speed variable:

```gdscript
@export var speed = 200
```

Then update your movement code to use that variable:

```gdscript
...

if Input.is_action_pressed("move_left"):
    direction.x = -1 * speed
if Input.is_action_pressed("move_right"):
    direction.x = 1 * speed
if Input.is_action_pressed("move_up"):
    direction.y = -1 * speed
if Input.is_action_pressed("move_down"):
    direction.y = 1 * speed

...
```

(The sign on each axis gets flipped according to the direction, but the magnitude now depends on a single `speed` value.) When you decide to adjust movement speed in the future, you only change one variable—either in your script or in the Godot Inspector.

## A Word of Caution

Now that you’ve seen how to optimize some parts of your code, you might feel a sudden urge to refactor everything at once. But don’t let this slow you down. In many projects, speed of iteration is as important as code quality—sometimes more so. Aim to get things working first; once the feature is stable and you understand it well, _then_ consider refactoring.

- **You don’t need to strive for perfection at every step.** Build and test features in whatever way makes sense to you. 
- **Refactor when it truly helps.** If you stumble onto a major improvement that simplifies your code without adding confusion, go for it. 
- **Don’t break your flow prematurely.** If you’re in the middle of prototyping, it might be best to hold off on deep refactoring until the idea is proven to work.

Ultimately, good development is about balance: keep your code healthy, but don’t let “perfectionism” hinder your progress.

## Conclusion

Refactoring is a skill: knowing _when_ and _how_ to improve your code can make you a more efficient and happier developer. Remember:
- Refactor when official practices recommend it, or when you see repeated code in too many places.  
- Export variables to simplify tweaks in the editor.  
- Don’t feel compelled to polish every line immediately; you can always clean up later.

As your project grows, you’ll develop your own sense of when to refactor. The more you experiment with different approaches, the better you’ll become at identifying what’s worth optimizing and what can wait. Good luck, and happy coding!

I'll see you in the next lesson!