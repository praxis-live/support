# Coding OpenGL video components

The `video:gl:p2d` and `video:gl:p3d` base components provide almost complete access to the underlying 2D or 3D OpenGL Processing API. _They can only be used when the video root renderer property is set to OpenGL_.

These components do not support the zero-copy and blend mode extensions from `video:custom`. Offscreen `PGraphics` are planned but currently not supported.

## Key method hooks

The primary method hooks you can override are -

 - `setup()` : called each time the pipeline is started and any time the code is updated. Because this method is called on code updates, unlike in Processing it needs to be suitable for real-time usage. Unlike other components, setup is called in the render cycle so like in Processing you can draw to the output.
 - `draw()` : called every render cycle. Draw to the output.
 
_Be aware that `setup()` and `draw()` are called in the rendering part of the processing cycle._ You should not send control signals during these methods. This also means that `setup()` is _not_ guaranteed to be called before fields / methods on your component are accessed by the update part of the processing cycle.

## Video input / output

All `video:gl:p2d` and `video:gl:p3d` components have an automatically generated video output port (`out`) on which drawing operations are performed. It is not currently possible to rename this port or add additional video outputs.

Video input ports can be created by annotating a `PImage` field with the [`@In`](annotations.md#in) annotation.

The `PImage` type in _PraxisLIVE_ is read-only and does not support any of the methods from Processing. It does have fields for width and height.

## Loading images

It is possible to load an image from a file by annotating a `PImage` field with the [`@P`](annotations.md#p) annotation. Note that in this case the field may be `null`.

All image loading is done aynchronously - the new image will be available sometime after the current video frame. To know when the image is loaded, or be informed of errors, use the [`@OnChange`](annotations-additional.md#onchange) and [`@OnError`](annotations-additional.md#onerror) annotations. _Note that methods called by these annotations are called in the update part of the processing cycle - you cannot draw in them, but you can send output_.

## OpenGL shaders

The `PShader` type allows you to filter video images using GLSL shaders.

This is an example base that provides a GLSL fragment shader property and applies that to an input image.

```java
@In(1) PImage in;

@P(1)
@Type.String(mime = GLSL_FRAGMENT_MIME, template = DEFAULT_FRAGMENT_SHADER)
@OnChange("resetShader")
@Port(false)
String fragment;

PShader shader;

public void setup() {
  resetShader();
}

public void draw() {
  if (shader == null) {
    shader = createShader(DEFAULT_VERTEX_SHADER,
       fragment.isEmpty() ? DEFAULT_FRAGMENT_SHADER : fragment);
  }
  shader(shader);
  updateUniforms();
  image(in, 0, 0);
  resetShader();
}

void resetShader() {
  shader = null;
}

void updateUniforms() {
  // set unforms in GLSL
  // eg. shader.set("uniform", 0.2);
}
```

See the [custom component library](https://github.com/praxis-live/pxg) for a growing range of OpenGL filter components that you can use and adapt.
