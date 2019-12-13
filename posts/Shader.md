## INTRODUCTION TO SHADERS

In the year 2000, Microsoft released Direct3D 8.0, which supported a new feature called **shaders**. Shaders are really powerful because they operate on GPU thereby eliminating the CPU for many tasks. 


> ### Shaders are basically nothing more than little programs that run directly on the GPU, thus leveraging even more of the GPU's power and moving more functionality away from the CPU.

   Initially shaders were very hard to program due to their syntax resembled the Assembly programming. Which lead to evaluation of HLSL & GLSL

- **High-Level Shader Language (HLSL)** : Microsoft recognised this shortcoming of Shaders Programming Language and in 2003 Microsoft released HLSL as a part of Direct3D 9.0. HLSL allowed the manipulation of shaders in a high-level language whose syntax was based on C.

- **OpenGL Shading Language (GLSL)**:  A year after in year 2004 OpenGL added support for Shaders with the released of GLSL in OpenGL 2.0.

- **OpenGL Embeded System Shading Language (OpenGL ES)**: Modern smart-phones such as the iPhone and Android-based phones all use OpenGL ES for interactive 3D graphics, which is an API for embedded systems based on, and very similar to OpenGL.

- **Web Graphics Library (WebGL):** cross-platform compatible 3D graphics API for the web browser. Programs embedded in JavaScript consist of shader code written in OpenGL ES Shading Language (GLSL ES).

### Quick Summary

- GLES is subset of OpenGL
- WebGL is subset of GLES
- WebGL 1.0 is based on OpenGL ES 2.0
- WebGL 2.0 is based on OpenGL ES 3.0


### Shader Types

With Direct3D 8.0 Microsoft announced two types of shaders namely vertex shaders and pixel shaders.

> #### **Vertex Shader is a GPU program that is executed once per vertex.**

**Vertex:** vertex is a data structure that describes attributes & the position of a point in 2D or 3D space.

Most attributes of a vertex represent vectors in the space to be rendered. These vectors are typically 1D (x), 2D (x, y), or 3D(x, y, z) dimensional and can include a fourth homogeneous coordinate (w). But a vertex can hold more contain attributes info like:

- **Position:** 2D or 3D coordinates representing a position in space

- **Color** Typically diffuse or specular RGB values, either representing surface colour or precomputed lighting information.

- **Reflectance** of the surface at the vertex, e.g. specular exponent, metallicity, fresnel values.

- **Texture coordinates** Also known as UV coordinates,these control the texture mapping of the surface, possibly for multiple layers.

- **normal vectors**These define an approximated curved surface at the location of the vertex, used for lighting calculations (such as Phong shading), normal mapping, or displacement mapping, and to control subdivision.

- **tangent vectors**These define an approximated curved surface at the location of the vertex, used for lighting calculations (such as Phong shading), normal mapping, or displacement mapping, and to control subdivision.

- **Bone weights**Weighting for assignment to bones to control deformation in skeletal animation.

- **Blend shapes**Multiple position vectors may be specified to be blended over time, especially for facial animation.

#### Example

            <!---SIMPLE VERTEX SHADER-->
            <script id="vertexShader" type="x-shader/x-vertex">
                void main() {
                    gl_Position = projectionMatrix *
                    modelViewMatrix *
                    vec4(position,1.0);
                    }
            </script>

> #### **Pixel or Fragment Shader is a 2D Shader program that is executed once per pixel to compute color and other attributes of each fragment.**

Fragment is the data necessary to generate a single pixel's worth of a drawing primitive in the frame buffer. Pixel shaders output one screen pixel as a color value.

#### Example

Variables used:

- **gl_FragCoord:** default **Input** holds the screen coordinates of the pixel or screen fragment that the active thread is working on. This variable is called _**varying**_ since it vary per thread.

- **u_resolution**: canvas size. its uniform across all threads hence called _**uniform**_
- **u_mouse**: x,y position of mouse.
- **u_time** : number of seconds since the shader started running.

- **gl_FragColor:** default **Output** gives the color Vec4(ARGB) of current pixel or screen fragment.


                    <!---SIMPLE FRAGMENT SHADER-->
                    <script type="x-shader/x-fragment" id="fragmentShader1">
                    //MACRRO https://www.programiz.com/c-programming/c-preprocessor-macros
                    #ifdef GL_ES
                    //Precison of floats variable
                    precision mediump float;
                    #endif
                    uniform vec2 u_resolution; // Canvas size (width,height)
                    uniform vec2 u_mouse; // mouse position in screen pixels
                    uniform float u_time; // Time in seconds since load

                    // time dependent color
                    vec4 getColor(){
                    return vec4(abs(sin(u_time)), 0.858,0.512,1.000);
                    }

                    // resolution dependent color
                    vec4 getColor2()
                    {
                        //** default input gl_FragCoord
                        // normalize the coordinate of the fragment to give value between 0-0-1.0
                        vec2 st = gl_FragCoord.xy/u_resolution;
                        return vec4(st.x,st.y,0.0,1.0);
                    }
                    void main() {
                    //reserved global variable = Vec4(ARGB)
                    vec4 result = getColor();
                    gl_FragColor =result; //  ** default output gl_FragColor
                    }
                </script>

### Quick Summary

- **2D shaders** act on digital images, also called textures, They modify attributes of pixels. eg: Fragment Shader
- **3D shaders** act on 3D models but may also access the colors and textures used to draw the model or mesh.eg Vertex, Geometry, Tessalation shader.
- **Vertex shaders** 3D shader modifying on a per-vertex basis of mesh but can't create new vertex.
- **Geometry shaders(Direct3D 10 and OpenGL 3.2)** Geometry shader programs are executed after vertex shaders. They take as input a whole primitive, possibly with adjacency information . It can generate new data for smoothening of curve.
- **Tessellation shaders** newer 3D shaders that act on batches of vertices all at once to add detail—such as subdividing a model into smaller groups of triangles or other primitives at runtime, to improve things like curves and bumps, or change other attributes. It can add more details near camera by subdividing on run time.



### References

- 🔗 https://thebookofshaders.com/01/
- 🔗 https://webglfundamentals.org/webgl/lessons/webgl-shaders-and-glsl.html
