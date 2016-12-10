# WebGL

## Rendering

In graphics, a virtual scene is described by:

* geometry
* viewpoint
* texture
* lighting
* shading

### Two Types of Rendering

* Software rendering - rendering computation done with CPU
* Hardware rendering - graphics computation done on GPU

Morever, there's server-based rendering and client-based rendering. WebGL uses client-based rendering.

## Coordinate System

Like any other 3D system, there are x, y, and z axes, where z signifies depth.
In WebGL, the coordinates are restricted to the range (-1, 1). Negative depth
signifies that the vertex is away from the screen.

### Vertices

A vertex is defined by three floating point numbers.

## Mesh

To draw 2D/3D objects, WebGL API provides `drawArrays()` and `drawElements()`
which accept a parameter called `mode` (restricted to points, lines, and
triangles).

So a mesh is a 3D object drawn using several primitive polygons.

## Vertex Shader

Performs:

* vertex transformation
* normal transformation
* texture coordinate generation
* texture coordinate transformation
* lighting
* color material application

## Fragment Shader (aka Pixel Shader)

Mesh is made up of multiple triangles, the surface is called a fragment, and the
fragment shader is responsible for calculating the color on individual pixels.

## OpenGL ES SL Variables

### Attributes

Hold input value of vertex shader program. Attributes point to the vertex buffer
objects that contain per-vertex data.

### Uniforms

Hold data common to vertex and fragment shaders

### Varyings

Pass data from vertex shader to fragment shader

## Graphics Pipeline

### In JavaScript

1. Initialize WebGL
2. Create arrays to hold the data representing the geometry
3. Convert arrays to buffers
4. Create and compile the shaders
5. Create the attributes and associate them with buffers

### Vertex Shader

When we start the rendering process `drawElements()` and `drawArray()`, the
vertex shader is executed for each point provided in vertex buffer object. It
calculates the position of each vertex and stores it in the varying gl_position,
and calculates color, texture coordinates, and vertices

### Primitive Assembly

Triangles are assembled and passed to the rasterizer

### Rasterization

Determining which pixels of the primitive will actually be rendered. Two steps:

1. Culling: All triangles with improper orientation, i.e. aren't visible in the
   view. Orientation is determined by the order of the vertices.
2. Clipping: If any part of the triangle is outside of the view area, it's
   removed

### Fragment Shader

Receives:

* Data from vertex shader from varyings
* Primitives from rasterization stage
* Calculates the color value for each pixel on triangle face

It stores the color values of every pixel in each fragment, which can be
accessed in fragment operations.






