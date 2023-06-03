---
layout: post
title:  "What are MonoBehaviours actually for"
date:   2021-06-03 11:00:00 +0200
categories: gamedev
---
# The most important building block on Unity?
When you begin learning to use Unity, you are told that MonoBehaviours are the basic building block on Unity. **The monolithical solution for any single problem you may ever have to solve**. It is only a matter of time, even a very short time, that you discover how using only MB makes the project a mess.

Everything runs independently, with unclear dependencies, and there's no way to know the order of execution. Adding new stuff becomes increasingly complex, and modifying anything generates unforeseen consequences. It is near impossible to apply good programming practices and architecture. 

In reality, MonoBehaviours are **a very specific tool that have two very specific use cases**, and an unnecessary but convenient third. And **the best principle is to use them as little as possible**, and only in the very specific cases for which they are actually built for. One of those use cases is learning to use Unity, when you're a newcomer and are still learning how to work with the engine. Once you have passed this first step, this case is not valid anymore.

Let's see how MonoBehaviours are actually built, and with that we will understand what problems do they solve.

    There are other tools provided by Unity: ScriptableObjects, GameObject, and the Transform component. But they won't be covered in this post.

# What they actually are

First of all, let's check their ancestry:

```MonoBehaviour > Behaviour > Component > UnityEngine.Object```

## Behaviour

Implements the

- **"enabled/disabled" module**.

## Component

Implements the:

- **Component system**:
    - GetComponent and public references to Unity's default components (rigidbody, collider, ...).
- **Component Message system**:
    - Broadcast/SendMessage.
- **Tag system**:
    - string tag, and CompareTag

## UnityEngine.Object

I'm including the namespace because It's important to differentiate this Object from C#'s System.Object.

Implements the:

- **Object lifecycle**: Instantiate, Destroy.
- **Hyerarchy lookup**: FindObject...
- **Operator overrides**: The infamous Unity's null overhaul, which overrides C#'s actual null definition. Further reading:
    - [https://blog.unity.com/technology/custom-operator-should-we-keep-it](https://blog.unity.com/technology/custom-operator-should-we-keep-it)
    - [https://docs.unity3d.com/Manual/NullReferenceException.html](https://docs.unity3d.com/Manual/NullReferenceException.html)
    - [https://forum.unity.com/threads/unity-future-net-development-status.1092205/page-2#post-7055404](https://forum.unity.com/threads/unity-future-net-development-status.1092205/page-2#post-7055404)
    - [https://forum.unity.com/threads/deprecation-of-operator-overload-and-fake-null-for-unityengine-object.1107368/](https://forum.unity.com/threads/deprecation-of-operator-overload-and-fake-null-for-unityengine-object.1107368/)

# What are the actually for

## 1st: Unity Engine messages:
They are the main way to communicate with the Unity Game Engine loops actually running our game.

```Awake, Start, OnEnable/Disable, Update, FixedUpdate, OnMouseEnter, OnPreRender, OnTriggerEnter, etc.```

I would also include the Behaviour's bool "_enabled_", as it controls whether the script instance is considered inside the loops.

## 2nd: Showing serialized variables in editor
This serves two purposes: 
- Dependency injection of references, made specially powerful by Unity's GUID system.
- Making easy to configure, as you don't need to change code. This simplifies iteration, as it allows to change values without recompiling.

## 3rd: Coroutines

Unnecesary as C# already implements async/await.

## (only temporary) 4th: Helping someone new to Unity
Even though they are not good solutions, and scale horribly, for a newcomer the MonoBehaviour class:
- Easily explains how to communicate with the Unity game loops (through the 1st use case).
- Provides a very visual way to instantiate scripts and cross-reference these instances (through the 2nd use case).