---
title: "Lecture15"
keywords: Lecture
tags: [Lecture]
permalink:  lecture15.html
summary: lecture15
layout: presentation
presentationTheme: '/assets/revealJS/css/theme/napier.css' 
---

Recommended Reading

.5

-   Artificial Intelligence for Games. Second Edition. Millington and
    Funge (2009).

.5 ![image](ai_book){width="\textwidth"}

Review -- State and State Machines
==================================

Review -- State Diagrams

.5

-   State modelling is one of (if not the) most important aspect of
    computing!

    -   Software development (object/component state).

    -   AI (state machines).

    -   Networking (protocol and hardware development).

    -   Software verification (state-space search).

-   Understanding and modelling application state is one of the most
    important skills and tasks you can do.

-   State modelling also provides dynamic behaviour.

.5 ![image](state_diagram){width="\textwidth"}

Review -- State Design Pattern

.5

-   The state design pattern allows us to encapsulate an object's state
    within another object.

-   We can switch the state object at any time during runtime --
    changing the behaviour of the object.

-   For example the ghosts in PacMan change behaviour.

-   Different behaviours are programmed in different objects -- the
    ghost simply calls the state class when it updates.

.5 ![image](state){width="\textwidth"}

Review -- AI Techniques

-   There are numerous usable AI techniques applicable to games
    development.

    -   Classical, deterministic techniques -- popular.

    -   Academic, non-deterministic techniques -- useful in some areas.

-   Different techniques accomplish different aspects of game behaviour.

    -   Movement.

    -   Decision making.

    -   Strategy.

    -   Learning.

-   Today we will look at the basics of decisions via state machines.

Example -- Batman: Arkham Asylum
================================

What are State Machines?

-   State machines (or specifically in our case Finite State Machines --
    FSM) are one of the most fundamental concepts and cornerstones of
    computer science.

-   A state machine is a technique of describing and modelling the state
    (e.g. behaviour, control, etc.) of a system in a mathematical
    manner.

-   The system is modelled with a number of states and the transitions
    between these states.

    -   The idea of a graph of states can come into play here --
        remember our description of a graph last week.

Example -- Batman: Arkham Asylum
[[Link]{style="color: blue"}](https://youtu.be/hNs-orQHaKU)

State Machines for AI
=====================

State Machines for AI

-   Let us return to the guard concept we presented last week.

-   We will take a simple view so we can just focus on state.

-   The guard has some basic actions:

    -   The guard patrols between point A and point B.

    -   If the guard is shot at, the guard will stop patrolling, engage
        the player, and fire back.

    -   If the guard loses sight of the player, the guard will return to
        patrolling between point A and point B.

    -   If the guard is hit, the guard will fall onto the ground and
        die.

State Machines for AI ![image](simple_state_guard){width="\textwidth"}

Example -- Maze Solving ![image](maze){width="\textwidth"}

Example -- Maze Solving

-   To solve a maze we can use a particular trick.

    -   This only works if the maze two or more ways in and out of the
        maze.

-   The basic algorithm is:

    -   Walk forward from the entrance until you hit a wall.

    -   Turn left.

    -   Now keep your right hand on a wall at all times.

    -   You will eventually reach the other exit.

        -   Although it will not necessarily be the fastest route.

Example -- Maze Solving ![image](maze_solve){width="\textwidth"}

State Machines in the Game Engine
=================================

State Machines in Our Game Engine

-   We will be implementing a basic, reusable state machine behaviour in
    our game engine.

    -   We want reusable so that it is simple for us to extend
        functionality if required.

-   We have already identified the state design pattern as a likely
    candidate for implementation of state machine behaviour.

-   What we need to do is implement this pattern in a manner that works
    in our game engine.

State Pattern in Our Engine
![image](game_state_pattern){width="\textwidth"}

`State` Interface

.5

-   The `State` interface only defines one method:

    -   `Execute`

-   This method executes the behaviour associated with that state upon
    the owner of the state.

    -   So the state needs to be told the entity to work on.

    -   Allows simple state reuse if this is desired.

.5 ![image](state_interface){width="\textwidth"}

`StateMachineComponent` Class

.5

-   The `StateMachineComponent` is a `Component` that we can attach to
    an `Entity`.

-   The class also follows the manager pattern -- it contains and
    manages a collection of states.

-   The core difference is that `Update` does not apply to all states,
    just the current state.

.5 ![image](state_machine){width="\textwidth"}

`Update`

-   `Update` is where the main functionality of the state machine
    occurs.

-   It is just one line of code:

    -   Execute the current state.

-   We call `ChangeState` to change the current state.

-   We call `Update` to execute the current state.

-   Although simple, the key work we have done is separate out and
    encapsulated the different object behaviours.

Decomposing State Machines
==========================

Decomposing State Machines

-   If a model has two or more properties it is worthwhile looking to
    see if they are independent.

-   If the properties are independent, it simplifies the logic to
    separate them into different state machines.

    -   You can do this -- just have two `StateMachineComponent`s
        attached to an `Entity`.

-   For example:

    -   A ranger wanders in the wilderness.

    -   If the ranger is hungry, the ranger eats.

    -   If it is night, the ranger lights a torch to see.

Decomposing State Machines
![image](ranger_all_states){width=".7\textwidth"}\
![image](ranger_decomposed_states){width=".7\textwidth"}

Comments on State Machines

-   FSMs are simple to use and understand.

    -   Advantageous in lots of circumstances.

    -   If you require degrees of intensity or "fuzziness" you will
        require a different AI technique.

-   FSMs are difficult to modify once in place.

    -   Small changes usually affect the entire FSM.

    -   You will generally need to rethink and rewrite your FSM code.

State Machines for Game Control

-   We can extend our state machine implementation to work as a game
    controller.

    -   All you need is an update and render for state, and call these
        when in the main game's relevant method.

-   This allows you to trivially implement game screens:

    -   Menu.

    -   Main gameplay.

    -   etc.

-   The main game only calls update and draw on these elements of the
    game based on the state.

-   This is effectively what the scene management system is doing.

Summary
=======

Summary

-   We've taken a broad look at state machines and how they work.

-   We have also taken a look at how we will implement them in our game
    engine.

-   Really, the ideas here will be more understandable when you
    implement the system and play around with the functionality.

-   This can be used to underpin much of the AI behaviour we will look
    at -- much like steering behaviours.
