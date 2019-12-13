## NOISE IN PROCEDURAL GENERATION


Nothing in the universe is static, universe is alwys under some random chaos but still there is a order in this Chaos. This imperfection in tecture is called Noise. 

> ### Noise is the randomness which create distinct pattern in real world materials.


In order to add this smooth randomness in a Shader we first need to generate noise in controlled way with the help of noise algorithms.

- 1 Dimentional Noise: reperesent Graphs
- 2 Dimentional Noise: Height Data for Terrain Generation
- 3 Dimentional Noise: Voulmetric Areas like Caves in 3D (Noise<0 Means Air)

###Noise Algos:
<table>
<thead>
<tr>
<th>Noise Algo</th>
<th align="center">Created by</th>
<th align="right">Year</th>
<th align="right">Link</th>
</tr>
</thead>
<tbody>
<tr>
<td>Classic Perlin noise</td>
<td align="center">Ken Perlin</td>
<td align="right">1983</td>
<td align="left"><a href="https://mrl.nyu.edu/~perlin/paper445.pdf">Paper</a> 
                  <a href="https://mrl.nyu.edu/~perlin/doc/oscar.html#noise">Oscar Source Code</a>
       </td>
</tr>
<tr>
<td>Simplex Noise</td>
<td align="center">Ken Perlin</td>
<td align="right">2001</td>
<td align="left"><a href="http://staffwww.itn.liu.se/~stegu/simplexnoise/simplexnoise.pdf">Paper</a></td>
</tr>
<tr>
<td>Open Simplex Noise</td>
<td align="center">Kurt Spencer</td>
<td align="right"></td>
<td align="left"><a href="https://uniblock.tumblr.com/post/97868843242/noise">Paper</a></td>
</tr>
<tr>
<td>Cellular Noise / Worley Noise</td>
<td align="center">Steven Worley</td>
<td align="right">1996</td>
<td align="left"><a href="http://www.rhythmiccanvas.com/research/papers/worley.pdf">Paper</a></td>
</tr>
</tbody>
</table>

In order to understand Noise Generation Algorithms we must understant the concept of Gradient

### Gradient:

**Derivative** is the rate of change of _𝑓(𝑥,𝑦)_, which can be thought of the slope of the function at a point (𝑥0,𝑦0).

The derivative of a function of a real variable measures the sensitivity to change of the function value (output value) with respect to a change in its argument (input value).

> Gradient is directional derivative which measures is the instantaneous rate of change of 𝑓(𝑥,𝑦) in the direction of the unit vector 𝑢.

The gradient is a fancy word for derivative, or the rate of change of a function. It’s a vector (a direction to move) that

- Points in the direction of greatest increase of a function.
- Is zero at a local maximum or local minimum (because there is no single direction of increase)


      Noise appears random, but isn't really. If it were really random, then you'd get a different result every time you call it.
     Instead, it's "pseudo-random" - it gives the appearance of randomness.

Noise is a mapping from Rn to R - you input an n-dimensional point with real coordinates, and it returns a real value. Currently the most common uses are for n=1, n=2, and n=3.

- The first is used for animation,
- the second for cheap texture hacks,
- and the third for less-cheap texture hacks.
- Noise over R4 is also very useful for time-varying solid textures, as I'll show later.

Noise is band-limited - almost all of its energy (when looked at as a signal) is concentrated in a small part of the frequency spectrum. High frequencies (visually small details) and low frequencies (large shapes) contribute very little energy. Its appearance is similar to what you'd get if you took a big block of random values and blurred it (ie: convolved with a gaussian kernel). Although that would be quite expensive to compute.

To make noise run fast, I implement it as a pseudo-random spline on a regular grid. Now we'll examine this fast implementation in some detail.

### The outline of Perlin's algorithm to create noise is very simple.

- Given an input point P, look at each of the surrounding grid points.
  In two dimensions there will be four surrounding grid points; in three dimensions there will be eight.
  In n dimensions, a point will have 2n surrounding grid points.

- For each surrounding grid point Q, choose a pseudo-random gradient vector G. It is very important that for any particular grid point you always choose the same gradient vector.

- Compute the inner product G . (P-Q). This will give the value at P of the linear function with gradient G which is zero at grid point Q.

- Now you have 2n of these values. Interpolate between them down to your point, using an S-shaped cross-fade curve (eg: 3t2-2t3) to weight the interpolant in each dimension. This step will require computing n S curves, followed by 2n-1 linear interpolations.

<iframe src="https://codesandbox.io/embed/perlin-vs-random-noise-ufftu?fontsize=14&view=preview" title="Perlin Vs Random Noise" allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

### References

- 🔗 Perlin Noise: https://en.wikipedia.org/wiki/Perlin_noise
- 🔗 https://web.archive.org/web/20071011035810/http://noisemachine.com/talk1/
- 🔗 Gradient Noise: https://en.wikipedia.org/wiki/Gradient_noise
- 🔗 Simplex Noise: https://en.wikipedia.org/wiki/Simplex_noise
- 🔗 Stefan Gustavson: http://staffwww.itn.liu.se/~stegu/
- 🔗 Simplex noise demystified: http://staffwww.itn.liu.se/~stegu/simplexnoise/simplexnoise.pdf
- 🔗 Cellular Noise: http://www.rhythmiccanvas.com/research/papers/worley.pdf
- 🔗 Symplectic Geometry: https://en.wikipedia.org/wiki/Symplectic_geometry
- 🔗 Simplex Noise Patent: https://patents.google.com/patent/US6867776
