## SHADERS PROGRAMMING

My [previous post](http:/hiteshsahu.com/blogs/shaders) was about Shader Languages & types of Shaders. In this post I gone use that theory to create a simple app using

- React.js as UI framework for syntactic sugar
- THREE.js for WebGL Geometry Generation


<iframe src="https://codesandbox.io/embed/simple-shader-example-7wnrf?fontsize=14&view=preview" title="Simple Shader Example" allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

#### Variables in Shader Programming :

#### 1) Uniform are variables that are uniform or common across all the GPU threads.

- They have same value on all Threads
- Used to communicate with your vertex or fragment shader from "outside"
- They can be read across all threads but cannot be changes.
- They are like static final variables in Java.
- Uniform are bridge between CPU & GPU
- Uniforms are defined with the corresponding type at the top of the shader right after assigning the default floating point precision.


            uniform vec3 iResolution;   // viewport resolution (in pixels)
            uniform vec4 iMouse;        // mouse pixel coords. xy: current, zw: click
            uniform float iTime;        // shader playback time (in seconds)

#### 2) Varyings are a variables a Vertex shader use to pass data to fragment shader.

- Verying vary from Thread to Thread.
- Uniform are bridge between Vertex shader & Pixel Shader


        <!-- Vertex Shader -->
        <script id="shader-vs" type="x-shader/x-vertex">

              // <-- Receive from WebGL application
              uniform vec3 vertexVariableA;

              // attribute is supported in Vertex Shader only
              attribute vec3 vertexVariableB;

              // --> Pass to Fragment Shader
              varying vec3 variableC;

        </script>

        <!-- Fragment Shader -->
        <script id="shader-fs" type="x-shader/x-fragment">

              // <-- Receive from WebGL application
              uniform vec3 fragmentVariableA;

              // <-- Receive from Vertex Shader
              varying vec3 variableC;

        </script>



#### Advance example

<iframe src="https://codesandbox.io/embed/shader-exploration-zjenw?fontsize=14&view=preview" title="Shader Exploration" allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>