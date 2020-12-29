# three-universal

Versions of [Three.js](https://github.com/mrdoob/three.js) compatible with Browsers, NodeJS, React Native Threads, etc by using [JSDOM](https://github.com/jsdom/jsdom) when needed.

This package provides the original builds and examples of three.js along with NodeJS specific builds and NodeJS specific examples.

## Example

Let's write a textured scene to a gltf file in the browser and using NodeJS.

### In browsers

It's totally equivalent to using the standard "three" library.

```javascript
import { GLTFExporter } from "three-universal/examples/jsm/exporters/GLTFExporter";
import { BoxBufferGeometry, MeshBasicMaterial, Mesh, Scene, TextureLoader } from "three-universal/build/three.module";

const exporter = new GLTFExporter();
const textureLoader = new TextureLoader();
textureLoader.load( `https://url-of-my-image.jpg`, function ( texture ) {

    const scene = new Scene();
    const box = new Mesh(
        new BoxBufferGeometry(),
        new MeshBasicMaterial( { map: texture } )
    );
    box.name = "box-test";
    scene.add( box );

    exporter.parse( scene, function ( gltf ) {

        downloadJSON(gltf);

    } );

}, undefined, function ( error ) {

    throw error;

} );
```

### On NodeJS

Here we can use the `file` protocol to load a local file, and fs-extra's `writeJson` to write the scene locally. We could also use a regular link
to load a remote image.

```javascript
import { GLTFExporter } from "three-universal/examples/node-jsm/exporters/GLTFExporter";
import { BoxBufferGeometry, MeshBasicMaterial, Mesh, Scene, TextureLoader } from "three-universal/build/three.module.node";
import { writeJson } from "fs-extra";

const exporter = new GLTFExporter();
const textureLoader = new TextureLoader();
textureLoader.load( `file://path-to-my-image.jpg`, function ( texture ) {

    const scene = new Scene();
    const box = new Mesh(
        new BoxBufferGeometry(),
        new MeshBasicMaterial( { map: texture } )
    );
    box.name = "box-test";
    scene.add( box );

    exporter.parse( scene, function ( gltf ) {

        const path = `path-to-my-local-file.gltf`;
        writeJson( path, gltf, {}, function ( data ) {
            console.log("Scene written!");
        } );

    } );

}, undefined, function ( error ) {

    throw error;

} );
```

## Unsupported DOM elements

So far, JSDOM doesn't support every object of the native DOM API. If one of the utils you intend to 
run transitively uses one object in the list below, you might encounter non-three.js related issues 
**when running on NodeJS only**.

List of currently unsupported DOM objects used by Three.js: 
<!-- listDomAutoGenerated --> 
-   ActiveXObject
-   AudioContext
-   DeviceOrientationEvent
-   ImageBitmap
-   TextDecoder
-   TextEncoder
-   WebGL2RenderingContext
-   WebGLRenderingContext
-   XRHand
-   createImageBitmap
-   orientation
-   webkitAudioContext
<!-- listDomAutoGenerated -->

## Sponsors

This development is open-sourced by [Sterblue](https://www.sterblue.com/). 

Join us at ![Sterblue Labs](https://labs.sterblue.com/)!

![Sterblue Labs](https://labs.sterblue.com/logos/Sterblue%20Labs-400w.png)!

## Development

When there is a new release of three at https://www.npmjs.com/package/three :

  - Copy the content of the `files` array in `utils/modularize.js` from three codebase to this package's (This could be avoided if we actually forked three JS instead of going for the current solution, see https://github.com/mrdoob/three.js/issues/20824#issuecomment-751663679 )
  - Update the version of the `three` dependency in this `package.json`
  - Update the version this package in this `package.json`
  - `npm install && npm run build && npm run test-e2e-node`
  - `npm publish`
