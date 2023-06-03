---
layout: post
title:  "A simple guide to clean game architecture"
date:   2021-06-23 11:00:00 +0200
categories: gamedev
---
# Break-down of the components of a software architecture

First of all, let's break down the four different components of a software architecture:

1. **Data.** Immutable data. *Assets, configurations, game design.*
2. **State.** Mutable data, current configurations and current states. *This is what goes into the saved game or the online messages.*
3. **Control.** Reads data and state to perform transformations to the state. (This can also be considered data and state).
4. **Display.** Show the data and state, or the difference between states. *Images, sound, text, logs, controller vibration, etc.*

And that's it. There's nothing else you need to be aware of.

# Causes of unnecessary complexity

The first thing that will create unnecessary complexity in your architecture is adding unnecesary data to an object's state. Any single extra bit doubles the complexity, so be wary of everything you add.

Different ways to avoid unnecessary complexity:

1. **Try to implement data and state with as basic the types as possible**. The ideal is to implement them fully with the basic types provided in the language. If the data and state are implemented with the most basic types available, it will be easier to modify or refactor both the information representation (data and state) and the operations (control and display). Most games tend to be arrays of ints, enumerations, and dctionaries with a string key. And then, arrays of these (entities, inventories, quest lists, maps).
2. **Rely on implicit information**. If you can infer a data point from other variables, there's no need to create a new variable. That variable is already there and therefore redundant. This is called duplicity. If the processing is costly, cache the result once calculated but make it obvious it is not a variable but an immutable inferred data point (read-only, hiding it behind a getter, records, etc).
3. **Keep the four different parts clearly separated between each other.** Mixing different code parts. Data and state, or state and control, or control and display.
4. **Separate based in the implementation, not on ontological entities.**  Mixing different object states with different control procedures. Indiscriminate implementation of OOP, trying to make everything generic, or mixing different code parts. Mixing diffferent programming parts because they feel ontologically related, forcing totally independent code together because in real life they are close. This is perhaps the most dangerous and widespread one. Equating the problem domain to both an object state and control operations, putting together variables and operations that don’t rely on each other in any way. This one is erroneously sprung by an indiscriminate implementation of OOP.

Now, let's look some examples.

# Examples

## First: Keep your state clean and separated

## Second: Don't mix different sections of your architecture

## Third: Know and respect the boundaries

## Fourth: Difference between implementation and ontological entities