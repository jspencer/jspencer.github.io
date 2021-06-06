+++
title = "Late Night Latches"
tags = ["basics", "latches", "schematic", "scr", "7432"]
category = ["electronics"]
+++


### The Setting

If you're like me, you'll enjoy a little late night soldering to relax and get away from screens for a while.

A good pairing I can recommend for this is to put on a YouTube video by [BigClive](https://www.youtube.com/user/bigclivedotcom). His videos are fun and informative, they are calmly presented, so you don't have to be watching the screen the entire time, and he has a great voice. You'll learn more about LEDs, power supplies, and ion generators than you ever thought you wanted to know.

### Latches

Latches are a basic building-block for computing. With a latch, you can build a flip-flop. With a bunch of those, you can build a register, SRAM, all kinds of good stuff.

It's fun to build things like this to get a feel for how they work in practice.

Here's a very simple latch using an SCR. (AKA [thyristor](https://en.wikipedia.org/wiki/Thyristor).)

![SCR Latch Schematic](/SCR-latch-schematic.png)

Except for maybe [one weird trick](https://hackaday.io/project/7975-one-transistor-latch), you can't make a latch using a single transistor. When you remove current from the base, current ceases to flow from the collector to the emitter.

An SCR, with an additional P-N junction, is a little different. You can apply a pulse to the gate, allowing current to flow through the component like a diode. The continuous current flow keeps the pathway open.

As you can see in the schematic, to reset this latch, you can break the current flow by pressing the normally closed switch. After you release the button, current won't flow until you trigger the gate again.

This is great for turning on and off this... I think it's a Christmas tree bulb. I'm not sure where this lamp came from.

![SCR Latch and Sky Bison](/SCR-latch-lamp.jpeg)

It's maybe not great for a digital circuit however. As soon as you remove the load, it unlatches.

For example, here's the same basic circuit with a novelty blinking LED. (I skimped on the switches. It was late, remember. Bare wires are switches.) 

![Appa watching a blinky](/SCR-latch-LED.jpeg)

This is an RGB LED that offers no control, it has internal circuitry that runs through a cycle of blinking color patterns.

When the LED reaches the end of its first cycle, **sometimes** it unlatches. It's unreliable. Sometimes the current is low enough that the path through the SCR can't be sustained. Maybe it depends on temperature, maybe there is variability in the timing of the LED patterns, who knows.

Let's try another latch.

![OR Gate Feedback Latch Schematic](/OR-gate-feedback-latch-schematic.png)

This one uses a single OR gate of a 74LS32. When we drive pin 1 positive, the gate turns on, and the feedback from the output to the second input keeps the gate open, and we have our latch. It's not shown on the schematic, but you can reset the latch by breaking the feedback connection.

![Sorry about the floating inputs](/OR-gate-latch.jpeg)

With this circuit, we can remove the load without changing the state of our latch.

One thing this latch also demonstrated is the importance of decoupling capacitors. (There's a 0.1µF capacitor behind the chip there, I didn't know I was going to be photographing this.) When I was testing the circuit on a breadboard I was getting false triggering when connecting power and other glitches. After adding the bypass cap, this issue went away. More on that [here](https://www.vagrearg.org/content/decoupling).

I should note that I'm a Software Engineer who likes to experiment, not an Electrical Engineer. Let me know if I've made any mistakes!

