# AFrame Tutorial

In this tutorial, we will create a AFrame Application.

You can read more about Aframe here: https://aframe.io
```
A-Frame is an open-source web framework for building virtual reality experiences. It is primarily maintained by Mozilla and the WebVR community.
```

Ready? Let's get started!

## Prereqs
* Browser
* [Http-server](https://www.npmjs.com/package/http-server)

You can follow along each part using the commit history

## [Part0] Setup
Aframe is really easy to setup. All you need to do is include the library at the header of your html file:
```
<script src="https://aframe.io/releases/0.8.0/aframe.min.js"></script>
```

## [Part1] Create a Scene

All of the components in AFrame need to be placed within an a scene. Go ahead and add a scene tag to the body:
```
<a-scene></a-scene>
``` 
Right now our scene is empty, but you should be able to pan around and see the cardboard viewer.

## [Part2] Box

The hello world of 3d projects is creating a cube, let's go ahead and do that!

All you need to do is add the box tag to your scene:

```
<a-scene>
    <a-box position="0 2 -5"
            rotation="45 45 0"
            color="blue"></a-box>
</a-scene>
```

Awesome! You can pan around and see the box :D

## [Part3] Sky

Next let's set a background sky. The sky takes a flat image and warps it around in a sphere surrounding the scene - making it look as if you are there!
You will need an equirectangular panorama for this (one is already provided in the assets folder) 

```
<a-sky src="url(/static-assets/sky.jpg)"
        phi-start="90"></a-sky>
```

Just like that you are able to add a 3d picture as the background of your scene.

## [Part4] Model

Let's replace our cube with a 3d model. A 3d model is represented with a .obj (object) and .mtl (material) file.
Checkout the blender example for how you can make it yourself: 

In this case we have a (very naive) R2D2 model that we want to place in the middle of our scene. Go ahead and replace the a-box with:
```
<a-entity position="2 -1 -5"
            rotation="0 45 0"
            obj-model="obj: url(/static-assets/r2d2.obj); mtl: url(/static-assets/r2d2.mtl);">
</a-entity>
```
You should see R2D2 floating in space!

## [Part5] Camera

Next let's add a camera. This will add head tracking so that when we look at it from a phone it will pan around. As well as keyboard traking if we want to move around in this world view:
```
<a-entity camera
            look-controls
            wasd-controls>
</a-entity>
```
A frame supports many different headsets, you can check out their documentation for more details

## [Part6] Animation

With aFrame it's really easy to add animation to your object, all you need to do is to add the a-animation tag.
Add this inside of the R2D2 entity tag to make him spin around!
```
<a-animation attribute="rotation"
    to="0 405 0"
    dur="5000"
    easing="ease"
    repeat="indefinite"></a-animation>
```
As you can see you can specify parameters such as rotate to, the duration, easing, repitition etc.

## [Part7] Cursor

Since this is a VR app, the user may not have a mouse to interact with the model. Instead, let's create a cursor.

Aframe provides a cursor utility that allows you to gaze and click on objects. Let's add this inside our camera:
```
<a-entity position="0 0 -3"
            geometry="primitive: ring; radiusOuter: 0.30;
                radiusInner: 0.20;"
            material="color: cyan; shader: flat"
            cursor="fuse: true">
</a-entity>
```
Note that we have to add this inside of the camera so that it keeps the position relative to the camera (where the person is looking)

## [Part8] Cursor Fuse

You may have noticed above that we have the fuse attribute set to true on the cursor. What this means is that the cursor can intersect with other objects in the scene. When it does it will begin fusing (sort of like starting a click). If the user holds the cursor in that position, then it will trigger a click. Add this animation inside of the cursor entity to show the fuse happening:
```
<a-animation begin="fusing"
    easing="ease-in"
    attribute="scale"
    fill="forwards"
    from="1 1 1"
    to="0.1 0.1 0.1"
    dur="1500"></a-animation>
```
As you can see, this animation starts when we the cursor begins fusing and will scale from 1 to 0.1 until it's done.

## [Part9] Cursor Click

When you hold a fused cursor for a while, it will trigger a click. Let's zoom the cursor back out when that happens:
```
<a-animation begin="click"
    easing="ease-in"
    attribute="scale"
    fill="backwards"
    from="0.1 0.1 0.1"
    to="1 1 1"
    dur="150"></a-animation>
```

## [Part10] Sound

Lastly, let's have R2D2 make a sound when we click on him! All you need to do is add this attribute to the model's entity:
```
sound="src: url(/static-assets/r2d2.mp3); on: click"
```
A frame will automatically set the sound source to where the model is located

## [Part11] AR

So far we have been working with Virtual Reality (you cannot see outside of the headset). Aframe can also work with Augmented Reality, overlaying models in the real world.

Follow this tutorial about how to add AR to AFrame: https://aframe.io/blog/arjs/
Checkout the last branch to see R2D2 over a hiro icon!
Note: the cursor doesn't really work well with the AR camera so it is removed here