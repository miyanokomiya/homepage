---
title: The state pattern pracitices for a diagraming app
date: 2023-11-Sa
slug: state-pattern-for-diagraming-app
categories:
  - tech
tags:
  - diagraming
thumbnailImage: /img/uploads/7b974cdc0713-20231124.png
---
I got inspired by this presentation (even didn't attend the event) and wrote down some ideas about a diagraming app that is kind of similar to what the presentation was featuring. We have obviously no definitive way to make them effectively though, the more knowledge and practices about these kind of apps we have, the better our apps must become. So, why not write down my experimental practices for anyone's sake?

https://speakerdeck.com/seanchas116/gong-tong-bian-ji-toroturunozuo-rifang

<!--more-->

## Where we are

There are a variety of applications we can make up with HTML Canvas. It means we can have at least as many variety of state pattern strategies as they are. Let's make things simpler a bit. What we'd like to make is a diagraming app.

I'm not going to talk about why we should follow the state pattern or what-the-heck-is-the-state-pattern kind of topics. The main theme of this article is how we practically apply the state pattern to our diagraming app.

To get familiar with it, in case you might not, I strongly recommend you to read this masterpiece article in advance. Making your own diagraming apps a few times from scratch is definitely the great way to grasp the broad idea about a diagraming app as well.

https://gameprogrammingpatterns.com/state.html

## What's our goal

What can we do via the app we are trying to make? Here's the list of them.

* Pan the viewport
* Select and deselect shapes
* Put new shapes
* Delete shapes
* Move shapes
* Draw a line
* Move a vertex of a line

Already looks intimidating, doesn't it? No? Cool! I guess implementing these features isn't actually the infeasible challenge for most of you. However, the important thing we need to consider from the beginning is **scalability**. The reason is obvious, right? I'm sure we never get satisfied with these features. We'll crave more and more features such as transforming, layout and snapping. We may want more variety of specific featured shapes such as a circle, an arrow and even a table.

What is **scalability**, by the way? It's definitely a controvertial word. Let's declare its meaning for this article. We can say our app has good scalability when the complexity of the app linearly increases as it gains new features. You may want me to answer what the complexity means and how we can measure it. Believe in your intinct.

## States

### Default state

Now we have the canvas in our app anyhow. What are we doing then? Nothing, of course. That is our default state. Leave this state until we need to get back here.

### Pan state

Let's begin from the most primordial feature: panning. Suppose we have an example shape in the canvas. What would we like to do next? We'd like to enjoy the liberty of observing the shape, of course. I'm a person advocating panning by the middle button dragging because I'm a big fun of Blender. Then, it appears we'd like to activate `pan state` on `middle button down`.

![](/img/uploads/7b974cdc0713-20231124.png)

Looks quite simple so far.

### Select state

How to name a state might be the toughest issue of us human beings. `pan` or `panning`, `select` or `selected` and so on. It's entirely up to your preferences though, I stick to the shortest form as much as possible this time just because it'll help me reduce my typos.

Well, we'd like to select a shape somehow. How? Left click, of course. Precisely, let us select a shape by `left button up` on the shape. Although `click` event is available to us, I'm not a big fun of handling this event directly with this kind of canvas app. It's also a bit awkward when we try to handle `down`, `up` and `click` harmoniously for whatever reason.

![](/img/uploads/19c82521298f-20231124.png)

Now, we got `select` state and moved to it. What is the state supposed to do then? It's a big topic as a matter of fact. The state should handle almost anything or at least as many as `default` state. It's a kind of intransient state.

What the heck is intransient state? Good question and there's no definitive definition for it since it just came up to my mind as I wrote this article. The idea is simple. A state is intransient if it handles some events to activate conventional operations.
What the heck are conventional operations then? There's no definitive definition as well though, I can list up some examples such as "panning", "(de)selecting shapes", "moving shapes", "undo/redo" etc. I hope you can see the idea from these examples.

Is `pan` state intransitive? No, it's transient. It could trriger some conventional operations if we really want it to do, but most of time we expect it just to pan the canvas and go back to the previous state as we release the button. Do something, then move to next state immediately, sounds transient, doesn't it? That's the idea.

What does intransitive state matter to us? It matters a lot. A lot and lot, because it has to handle a lot of events for conventional operations. Honestly, I don't have great answer for how to effectively implement intransient states. Class oriented strategy may work to unify a great part of those states, even though I'm basically a class hater in JS realm. I'm not going to elaborate the detail about the implementations here, it's would be a another huge topic.

Put the implementations aside or forward, maybe behind, it appears some states such as `pan` state should be activated from any intransient states. Then, we can make our diagram a bit more general and simpler like this.

![](/img/uploads/44ee1297eb4e-20231124.png)

So far so good. Thanks to the idea of the intransient state, we can extract some conventional transitions from each state. It'll make our state diagram lovely.

### Put new shapes

Sadly, things become complicated from now on, but it also mean we are now getting into more practical operations. First of all, we'd like to put a new shape into the canvas.

Let's define the process for putting a new shape to begin with. Suppose there's a button for a shape in our app, and we can put a new shape by drag & drop operation from the button to the canvas.

One issue was detected. I said there's a button in our app. It's a button, it's a HTML button element. Do our states also need to handle any events taking place outside the canvas element? Partially yes. We sometimes want our states to handle certain custom events and this is one of those cases.

In short, we'd like to activate `put shape` state by `left button down` on the button. Since this `left button down` takes place outside the canvas, we'd better make up a custom event for this feature. Otherwise, we would end up making tons of custom events for each UI outside the canvas.

This time, we're going to make up `activate state` event that contains some parameters to activate certain state. One notice is that whether the certain state is activated or not is still up to the current state. If the current state doesn't handle the event, just nothing happens then.

The event looks like this for example. It would change depending on an implementation.

```ts
{ type: "activate-state", data: { name: "put-shape", shapes: [..] } }
```

Which state would we like to activate 'put shape' from? We have had the answer, of course. It should be any intransient states because the new shape button should be available to us whenever we are in an intransitive state. After putting a new shape on the canvas, probably we'd like to select the newly created shape for further operations. So `put shape` state is a transient state and let us move to `select` state consecutively.

![](/img/uploads/0a037745ebc9-20231124.png)

We also need to save and select the new shape as we finish `put shape` state. To make things simple, we are mainly focusing at the transition between states.

### Delete shapes

We already have enough to tools to achieve this feature. Let's say we'd like to delete selected shapes by the event `"Delete" key press`. Maybe `release`, whatever you like.

Since we have `select` state that should be the one handling this operation.

![](/img/uploads/4675fd8453f1-20231124.png)

As long as `select` state checks shapes' selection status correctly, all we need to do is just adding the handling for the event `"Delete" key press`. Then, `select` state takes care of the state transition as shapes' selection status changes.

### Move shapes

Almost likewise `put shape` state, but the activation trigger is a bit different and should take place from `select` state.

![](/img/uploads/23c886d1c641-20231124.png)

Things are going easy, right? Then, let's make them a bit complicated because we want our app to be convenient, don't we?

Imagine, we'd like to move a shape that isn't currently selected. Up to now, the only way to activate `move` state requires us to select the shape in advance, then start dragging the shape. That means we have to press the button twice. We don't want to bother to press it twice, let us move any shape by just dragging regardless of its selection status. It seems we want below state transition.

![](/img/uploads/0e2012398451-20231124.png)

Wait, we already have this transition and `default` state is one of intransient states, so we these triggers are competing each other.

![](/img/uploads/0a8259641d20-20231124.png)

It might be a good idea just to move to `move` state, but probably we would end up facing some troubles. We usually expect specific appearance during moving shapes such as cursor style and the bounding box. It doesn't feel good if those specific appearances momentarily show up every time we select a shape. Therefore, we need to add branches to activate either `select` or `move` state by the event `left button down` on a shape.

Either way, we do `left button down` on a shape at first. Then, if we drag, the state should change to `move` state. If we don't drag and release the button, the state should change to `select` state. It seems we need to have a flag to track if we hold the button and wait the next action while the flag is on.

No way. We are practicing the state pattern, so why not making a state instead of the flag? Let's name it `left down on shape` state. I know it's long, you can name it as you like.

![](/img/uploads/9b13dfec368e-20231124.png)

We don't need to bother to draw a flowchart for the flag because the dedicated state presents the simple transition. Even the role of this state feels a bit special, the state does nothing special but handling some events, `left button up` and `mouse move`, like other states. So there's no new idea for this transition, we just added a state and let it handle some events.

Now, We've got a great insight from this example. Anytime we feel like having a flag to handle something, we'd better consider if we can achieve it by adding a state without introducing the flag into currently existing states. When the number of those flags isn't large, we may feel we can handle it somehow. However, it's never the case as the number of them increases over the time.

### Draw a line

Did you remember we are making a diagraming tool? We can now create, select, move and delete a shape. Let's get a line then. I suppose we'd like to draw a line by following steps.

1. Press a button to get ready to draw a line
2. Do `left button down` to start drawing the line

   * The first vertex is placed by this step
3. Do `mouse move` to move the second vertex
4. Do `left button up` to finish drawing the line

   * We'd expect to select the newly created line

As you see, it appears we need two new states to achieve these steps. Let's name them `line ready` state and `line draw` state. Likewise `put shape` state, we should be able to activate the first state anytime the button is available to us. That means the state should be able to be activated from any intransient states.

![](/img/uploads/0d3639f18c6b-20231124.png)

Although the whole transitions look bigger since four different states are related to this operation, each transition is pretty straightforward as a matter of fact. It's basically just one-way transition and all events for the transitions are already familiar to us.

### Move a vertex of a line

Might as well, we feel like moving a vertex! Problem detected, which state can we activate `move vertex` state from? The activation event is surely `left button down` on a vertex, it seems easy enough. We can suppose that the line we are about to edit should be selected in advance. A line is certainly a kind of shapes, so that we should be in `select` state when a line is selected.

Although things become complicated once again, we know what we should rely on to begin with. That's right, make a state to handle complexity we are facing to.

![](/img/uploads/4c4a85f8d92b-20231124.png)

How's this? It would work, but we should remind ourselves that `intransient` in the image represents all intransient states. The number of `select xx` states would also increase because we need new one every time we try to implement specific operations for a specific shape type. Therefore, the relationships between intransient states and `select xx` states would explode over the time.

One way I can suggest is having a hub state to handle selection branches line this.

![](/img/uploads/b33cc5a0bfad-20231124.png)

I put `any state` before `selection hub` state because this is the real situation we are facing to. `selection hub` state is activated from technically any states that can change shapes' selection status. I've avoided mentioning multiple shape selection so far to make things simple, but it does matter to handling `select xx` states.

This is a good occasion, let's have `select multiple` state now. Suppose we selected a line and another conventional shape, so we are in `select multiple` state. Next, we deselected the conventional shape. Where are we then? We should be `select line` state for sure. I'm sorry I already don't want to bother to draw this situation without having `selection hub` state. So, this is the result.

![](/img/uploads/e593a4198a05-20231124.png)

Since `any state` represents literally any states, this image isn't as simple as it looks like. However, it's also not as complicated as you might imagine it is. Aside from `selection hub` state, all other states just have to activate `selection hub` state as shapes' selection changes anyhow. As long as we manage to cram all selection branches into the hub state, it'll take care of all complexity of selection transitions. Each branch wouldn't be so complicated to deal with though, once they spread throughout our states it's gonna be a disaster. So, let's cram them all into the hub state. The day is saved.

We've got a great tool, then let's get back to the first place. What we need remained is just one more state: `move vertex` state. It's almost identical to `draw line` state though, the situation is a bit different from its situation. So, making new state is simpler than taking care of minor differences of them in one state.

![](/img/uploads/45bbf7ed2fa9-20231124.png)

## Wrap up

Thanks for reading all the way up to here. We've got through all features we expected. I hope this article gives you some sort of insight about the state pattern for diagraming tools. These are literally the very first steps to sculpture your ideal tool and there're still tons of topics yet to talk about. You may have much better strategy than this article. Congrats, share it with me discreetly, please!

Have a nice diagraming day 👋