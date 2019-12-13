# Ammo.js vs Cannon.js vs Physics.js vs Matter.js

> ## A physics engine is middleware used in video games, animations & scientific simulation to provides simulation of physical world.

A typical Physics Engine can simulate:

- Rigid body dynamics
  eg: [collision detection](http://kripken.github.io/ammo.js/examples/webgl_demo_softbody_rope/index.html),
- Soft body dynamics 
  eg: [cloaths](http://kripken.github.io/ammo.js/examples/webgl_demo_softbody_volume/index.html)
- Fluid dynamics 
  eg: [water and Brownian motion](https://david.li/fluid/)

## On accuracy of simulations with real world Physics Engines can be brodly sub divide into two categories:

1. **Real-time Physics Engines**: fast but somehow inaccurate. Commonly used in Video Games.

<div align="center">
 <img src="https://media.giphy.com/media/GtxeEVz146f2o/giphy.gif" alt="Fail Physics" style="width:100%;">
</div>
   Examples of commonly used Real Time Physics Engines:

<ul>
<li><a href="https://www.geforce.com/hardware/technology/physx" title="PhysX">NVIDIA PhysX</a> - Nvidia's GPU bases Physics Engine. Used extensibly in modern video games</li>
<li><a href="https://github.com/bulletphysics/bullet3" title="Bullet (software)">Bullet Engine3</a> - Real-time c++ collision detection and multi-physics simulation for VR, games, visual effects, robotics, machine learning etc. </li>
<li><a href="https://projectchrono.org/" title="Project Chrono">Project Chrono</a> - An open source Powerful simulation engine for multi-physics applications in industrial simulations. This can simulate wheeled and tracked vehicles operating on deformable terrains, robots, mechatronic systems, compliant mechanisms, and fluid solid interaction phenomena. Systems can be made of rigid and flexible/compliant parts with constraints, motors and contacts; parts can have three-dimensional shapes for collision detection.</li>
</ul>

Other Open source Real Time Physics Engines

<ul>
<li><a href="http://asl.org.il/" title="Advanced Simulation Library">Advanced Simulation Library</a></li>
<li><a href="https://box2d.org/" title="Box2D">Box2D</a></li>
<li><a href="https://chipmunk-physics.net/" class="mw-redirect" title="Chipmunk physics engine">Chipmunk physics engine</a></li>
<li><a href="http://newtondynamics.com/forum/newton.php" title="Newton Game Dynamics">Newton Game Dynamics</a></li>
<li><a href="https://www.ode.org/" title="Open Dynamics Engine">Open Dynamics Engine</a>
</li>
</ul>

2. **High Precision Physics Engines**: slow and requires more processing power. Used in scientific simulation and Hollywood movies.

   Examples of High precision Physics Engines:

<ul>
<li><a href="https://web.solidthinking.com/vissim-is-now-solidthinking-embed" title="PhysX">VisSim</a> VisSim is a visual block diagram program for simulation of dynamical systems and model based design of embedded systems. VisSim is used in control system design and digital signal processing for multidomain simulation and design</li>
<li><a href="http://www.design-simulation.com/" title="Bullet (software)">Working Model</a>This program will simulate the interaction of the model's parts such as springs, ropes, and motors are combined with objects in a 2D working space & can also graph the movement and force on any element in the project. It is useful for basic physics simulations, and can be quite a powerful dynamic geometric analytical tool, </li>
</ul>

## Comparision of Game Physics engines for JavaScript

In JavaScript we have choice of **AmmoJS**, **CannonJS**, **PhysicsJS** & **MatterJS**

**Ammo.js** is a direct port of the **Bullet3** physics engine to JavaScript, using **Emscripten**. The source code is translated directly to JavaScript, without human rewriting, so functionality should be identical to the original Bullet3 engine. "Ammo" literally stand for

> ### "Avoided Making My Own js physics engine by compiling bullet from C++"

**Ammo.js** is powerful but have a horrible documentation.

Unlike **Ammo.js**, **Cannon.js** is written in pure JavaScript from the start by [Stefan Hedman(schteppe)](https://github.com/schteppe). It have rather descent [Documentation](http://schteppe.github.io/cannon.js/docs/).

**Matter.js** another alternative which is derived from **Physics.js**. **Physics.js** is deprecated now in favour of **Matter.js**. **Matter.js** is quite capable of simulatating physics in 2D models.

## Comparision Table:

<table> <thead> <tr><td></td><td><a target="_blank" href="https://github.com/kripken/ammo.js">ammo.js</a></td><td><a target="_blank" href="https://github.com/schteppe/cannon.js">cannon</a></td><td><a target="_blank" href="https://github.com/wellcaffeinated/PhysicsJS">physics.js</a></td><td><a target="_blank" href="https://github.com/liabru/matter-js">Matter.js</a></td></tr></thead> <tbody> <tr> <td>stars 🌟</td><td>1775</td><td>2342</td><td>3244</td><td>8580</td></tr><tr> <td>forks 🍽</td><td>241</td><td>351</td><td>395</td><td>930</td></tr><tr> <td>issues ⚠️</td><td>95</td><td>160</td><td>59</td><td>204</td></tr><tr> <td>updated 🛠</td><td>Apr 29, 2019</td><td>Mar 1, 2019</td><td>Mar 27, 2019</td><td>Apr 5, 2019</td></tr><tr> <td>created 🐣</td><td>May 29, 2011</td><td>Jan 4, 2012</td><td>Mar 26, 2013</td><td>Feb 19, 2014</td></tr><tr> <td>size 🏋️&zwj;♀️</td><td> <a href="https://bundlephobia.com/result?p=ammo.js" target="_blank"><img src="https://flat.badgen.net/bundlephobia/minzip/ammo.js" alt="Minified + gzip package size for ammo.js in KB"></a> </td><td> <a href="https://bundlephobia.com/result?p=cannon" target="_blank"><img src="https://flat.badgen.net/bundlephobia/minzip/cannon" alt="Minified + gzip package size for cannon in KB" ></a> </td><td> <a href="https://bundlephobia.com/result?p=physicsjs" target="_blank"><img src="https://flat.badgen.net/bundlephobia/minzip/physicsjs" alt="Minified + gzip package size for physicsjs in KB"></a></td><td><a href="https://bundlephobia.com/result?p=matter-js" target="_blank"><img src="https://flat.badgen.net/bundlephobia/minzip/matter-js" alt="Minified + gzip package size for matter-js in KB" ></a></td></tr><tr> <td>Written in</td><td>Port of the c++ <a target="_blank" href=" https://github.com/bulletphysics/bullet3"> Bullet physics engine</a> using emscripten</td><td>Pure JavaScript</td><td>Pure JavaScript</td><td>Port of Physics.JS, JavaScript</td></tr><tr> <td>Pros</td><td> Powerful, more features, production ready, can write code in C++</td><td>Light weight, can perform 3D rigid-body dynamics, discrete collision detection, cloth simulation & Gauss-Seidel constraint solver.</td><td>Light weight 2D game engine</td><td>Powerful, Lightweight 2D game engine, Good Documentation</td></tr><tr> <td>Cons</td><td>Big Size 300kb</td><td>Not complete. not sufficient for X3DO, lots of TODOs </td><td>Deprecated and replaced by <a target="_blank" href="http://brm.io/matter-js/">Matter.js</a>, 2D only </td><td>2D only</td></tr><tr> <td>Demo</td><td> <a target="_blank" href="http://kripken.github.com/ammo.js/examples/webgl_demo/ammo.html">Cubes,</a><a target="_blank" href="http://kripken.github.com/ammo.js/examples/webgl_demo/ammo.wasm.html">Cubes (WebAssembly), </a><a target="_blank" href="http://kripken.github.com/ammo.js/examples/webgl_demo_softbody_rope/index.html"> SoftBody-Rope, </a><a target="_blank" href="http://kripken.github.com/ammo.js/examples/webgl_demo_softbody_cloth/index.html"> SoftBody-Cloth, </a><a target="_blank" href="http://kripken.github.com/ammo.js/examples/webgl_demo_softbody_volume/index.html"> SoftBody-Volume, </a><a target="_blank" href="http://kripken.github.com/ammo.js/examples/webgl_demo_terrain/index.html">Heightmap , </a> <a target="_blank" href="http://kripken.github.io/ammo.js/examples/webgl_demo_vehicle/index.html">Vehicle</a></td><td><a target="_blank" href="http://schteppe.github.io/cannon.js/">Cannon.js demo</a></td><td><a target="_blank" href="http://wellcaffeinated.net/PhysicsJS/">Physics.js demo</a> </td><td><a target="_blank" href=" http://brm.io/matter-js/demo/#mixed">Matter.js demo</a> </td></tr></tbody></table>

## Conclusion:

Choice of game engine is clearly depends upon your needs for example if you want to build a 2D game than **Matter.JS** is more suitable which is developed on top of **Physics.js** and have a zip size of hardly **25kb**. List of supported features by Matter.js:

- Rigid bodies
- Compound bodies
- Composite bodies
- Concave and convex hulls
- Physical properties (mass, area, density etc.)
- Restitution (elastic and inelastic collisions)
- Collisions (broad-phase, mid-phase and narrow-phase)
- Stable stacking and resting
- Conservation of momentum
- Friction and resistance
- Events
- Constraints
- Gravity
- Sleeping and static bodies
- Plugins
- Rounded corners (chamfering)
- Views (translate, zoom)
- Collision queries (raycasting, region tests)
- Time scaling (slow-mo, speed-up)
- Canvas renderer (supports vectors and textures)
- [MatterTools](https://github.com/liabru/matter-tools) for creating, testing and debugging worlds
- World state serialisation (requires [resurrect.js](https://github.com/skeeto/resurrect-js))
- Cross-browser and Node.js support (Chrome, Firefox, Safari, IE8+)
- Mobile-compatible (touch, responsive)
- An original JavaScript physics implementation (not a port)

If you want to build a 3D game than you have 2 options: **Ammo.js** & **cannon.js.**. The **Ammo.js** is derived from **Bullet3** Game engine. **Bullet3** Game engine is written is C++. **Ammo.js** make use of **Emscripten** which is an LLVM-to-JavaScript compiler. That means **Emscripten** takes LLVM bitcode - which can be generated from C/C++, using llvm-gcc (DragonEgg) or clang, or any other language that can be converted into **LLVM** - and compiles that into JavaScript, which than can be run on the web (or anywhere else JavaScript can run). _LLVM_ is a whole different topic to talk about and I gone cover it in some other post.

Back to **Ammo.js**, the direct derivation of **Ammo.js** from a C++ game engine makes is extreamly powerful but also increases its package size to **300kb**. This big package size can be reduced by removing uneeded interfaces from **ammo.idl** but thats just another overhead. There are lots of game which use **Ammo.js**.

For 3D game engine **Cannon.js** is an alternative to **Ammo.js**. **Cannon.js** is written entirely in JavaScript and take advantages of JavaScript Prototype. its light weight **35 kb** and do same things as **Ammo.js**. Its sufficient for game designing on web.

In comparison with **Ammo.js**, **cannon.js ** is compact, more comprehensible, more powerful with regard to its performance and also easier to understand, but did not have as many features.

Doing a lot of physics calculation on UI thread can have hudge hit on performance. This can be reducd by moving [Cannon.js to a Service Worker as described here](https://github.com/schteppe/cannon.js/blob/master/examples/worker.html).

There are couple of more Physics Engines availble like [**Energy.js**](https://github.com/samuelgirardin/Energy.js) & [**Oimo.js**](https://github.com/lo-th/Oimo.js/) as alternative to _Cannon_ and _Ammo.js_. _Ohimo.js_ is looking promising but this engines are relatively new and lot of work needed to be done. Thats all pretty much we have for Physics simulation in javaScript for now.

> In Summary every javascript 3D physics engine lacks one thing or another. What do you think?
> How was your experience with Physics Engines? please leave in comment.

