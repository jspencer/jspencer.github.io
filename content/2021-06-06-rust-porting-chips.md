+++
title = "Porting Rust Microtetris"
tags = ["rust", "embedded", "stm32", "microcontroller"]
category = ["rust"]
+++

I wanted to try [Marek Miettinen's microtetris](http://marekmiettinen.fi/micropong/index.html) but I didn't have a Cortex-M0 chip (ThumbV6). I decided to try porting it to M4 (ThumbV7).

I was impressed with how straightforward this was. After a bit of a discussion with the compiler, everything worked on the first try. The architecture and peripherals of the two chips are very similar, so it was not a huge deal, but as you can see, [the diff](https://github.com/jspencer/micropong/commit/421ccc1860495c121f5b9a3d3de9bef7eb9cc756) is not trivial either.

I didn't spend any time debugging. With so many of the invariants specified using the Rust type system, I didn't need to. I probably did spend more time getting it to compile than I would have done in C, but once I flashed it to the device, I was directly on to thinking about improvements. 

This code only polls for button presses during part of the game loop, and the controls feel a bit unresponsive. It could probably do with an interrupt routine just for the controls. Doing so would probably require adding explicit button debouncing.

The point is, if I had been doing this in C I surely would have missed something. I'd be spending this time debugging some code I had not written and had never seen run correctly.

Don't get me wrong, there are still plenty of ways to write embedded code in Rust that doesn't work. For example, I had many builds of [this](@/2021-05-01-making-custom-i2c-displays.md) that just plain did nothing. There are also always going to be those times when you're fiddling with the hardware for what will turn out to be a software problem, and vice versa.

There are other rough edges. The generated code constraining these features is pretty cumbersome. For more on that, see the example in therealprof's post on [arbitrary integer primitives](https://therealprof.github.io/blog/roadmap-2021-arbitrary-size-primitives/).

In general however, I think this type of reduction in friction is really important. I spend a lot of time in large codebases, where you spend much more time debugging and managing interactions with existing code than you do writing truly new code. The more that your tools can help ensure the continued correctness of what you have already written, the better. It frees up time and gives you the confidence to change and improve things in ways you would have otherwise avoided.

Check out the code [here](https://github.com/jspencer/micropong).

Further reading: I came across [this post](https://www.ecorax.net/as-above-so-below-1/) by Pablo Mansanet that has a similar sentiment and goes into much more detail.

