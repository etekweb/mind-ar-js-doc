---
id: threejs-face-facemesh
title: ThreeJS FaceMesh
sidebar_label: ThreeJS FaceMesh
---

import {customFields} from '/docusaurus.config.js';
import useBaseUrl from '@docusaurus/useBaseUrl';

FaceMesh effect

## Try it out
![img](/img/demo/face-mesh-demo.gif)

<a href={useBaseUrl('/face-tracking-samples/three-facemesh.html')} target="_blank">Live Demo</a>

<code>
{`
<html>
  <head>
    <script src="https://cdn.jsdelivr.net/npm/mind-ar@${customFields.libVersion}/dist/mindar-face-three.prod.js"></script>
    <script type="module">
      const THREE = window.MINDAR.FACE.THREE;
      const mindarThree = new window.MINDAR.FACE.MindARThree({
	container: document.querySelector("#container"),
      });
      const {renderer, scene, camera} = mindarThree;
      const light = new THREE.HemisphereLight( 0xffffff, 0xbbbbff, 1 );
      scene.add(light);
      const faceMesh = mindarThree.addFaceMesh();
      const texture = new THREE.TextureLoader().load('./assets/canonical_face_model_uv_visualization.png');
      faceMesh.material.map = texture;
      faceMesh.material.transparent = true;
      faceMesh.material.needsUpdate = true;
      scene.add(faceMesh);
      const start = async() => {
	await mindarThree.start();
	renderer.setAnimationLoop(() => {
	  renderer.render(scene, camera);
	});
      }
      start();
    </script>
    <style>
      body {
	margin: 0;
      }
      #container {
	width: 100vw;
	height: 100vh;
	position: relative;
	overflow: hidden;
      }
    </style>
  </head>
  <body>
    <div id="container">
    </div>
  </body>
</html>
`}
</code>
