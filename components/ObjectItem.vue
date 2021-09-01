<template>
  <div class="object-item-wrapper">
    <div
      class="object-item-content"
      ref="objectItem"
      @mousemove="rotateObjectItem"
      @mouseleave="resetObjectItem"
      @click="openObjectViewerPage"
    >
      <canvas class="object-item-canvas" ref="renderCanvas"></canvas>
      <h2>{{ objectData.itemTitle }}</h2>
      <p>{{ objectData.itemSubtitle }}</p>
    </div>
    <div
      class="object-viewer-dialogue-wrapper"
      ref="objectViewerDialogueWrapper"
    >
      <div
        class="object-viewer-dialogue-container"
        ref="objectViewerDialogueContainer"
      >
        <div class="object-viewer-dialogue-title-wrapper">
          <h2>{{ objectData.itemTitle }}</h2>
          <p>{{ objectData.itemSubtitle }}</p>
        </div>
        <canvas
          class="object-viewer-dialogue-canvas"
          ref="objectViewerDialogueCanvas"
        ></canvas>
        <div class="object-viewer-dialogue-buttons">
          <button
            class="close-object-viewer-dialogue-button"
            @click="closeObjectViewerPage"
          >
            Close
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import * as gsap from "gsap";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import * as THREE from "three";
import * as dat from "dat.gui";
export default {
  props: {
    objectData: Object,
    controls: null,
  },
  data() {
    return {
      targetCanvas: HTMLCanvasElement,
      scene: THREE.Scene,
      camera: THREE.PerspectiveCamera,
      renderer: THREE.Scene,
    };
  },
  methods: {
    rotateObjectItem(event) {
      const cursor = {
        positionX:
          ((event.pageX - this.$refs.objectItem.offsetLeft) /
            this.$refs.objectItem.clientWidth -
            0.5) *
          0.1,
        positionY:
          -(
            (event.pageY - this.$refs.objectItem.offsetTop) /
              this.$refs.objectItem.clientHeight -
            0.5
          ) * 0.1,
      };
      this.$refs.objectItem.style.transform = `perspective(1em) rotateX(${cursor.positionY}deg) rotateY(${cursor.positionX}deg) scale(1.02)`;
      this.$refs.objectItem.style.setProperty(
        "--shine-x",
        `${event.pageX - 200 - this.$refs.objectItem.offsetLeft}px`
      );
      this.$refs.objectItem.style.setProperty(
        "--shine-y",
        `${event.pageY - 5 - this.$refs.objectItem.offsetTop}px`
      );
    },
    resetObjectItem() {
      this.$refs.objectItem.style.transform = `perspective(1em) rotateY(0deg) rotateX(0deg)`;
    },
    openObjectViewerPage() {
      this.targetCanvas = this.$refs.objectViewerDialogueCanvas;
      this.renderer = new THREE.WebGLRenderer({
        canvas: this.targetCanvas,
        antialias: true,
      });
      this.renderer.shadowMap.enabled = true;
      this.renderer.shadowMap.type = THREE.PCFShadowMap;
      this.renderer.setClearColor(0x000000, 0);
      this.renderer.setSize(474, 294);
      this.controls = new OrbitControls(this.camera, this.targetCanvas);
      gsap.gsap.to(this.$refs.objectViewerDialogueWrapper, {
        visibility: "visible",
        duration: 0.1,
        ease: "power3.out",
      });
      gsap.gsap.to(this.$refs.objectViewerDialogueContainer, {
        opacity: 1,
        scale: 1,
        duration: 0.5,
        ease: "power3.out",
      });
    },
    tick() {
      this.controls.update();
      this.renderer.render(this.scene, this.camera);
      window.requestAnimationFrame(this.tick);
    },
    closeObjectViewerPage() {
      this.targetCanvas = this.$refs.renderCanvas;
      this.renderer = new THREE.WebGLRenderer({
        canvas: this.targetCanvas,
        antialias: true,
      });
      this.renderer.shadowMap.enabled = true;
      this.renderer.shadowMap.type = THREE.PCFShadowMap;
      this.renderer.setClearColor(0x000000, 0);
      this.renderer.setSize(
        this.targetCanvas.clientWidth,
        this.targetCanvas.clientHeight
      );
      this.controls = new OrbitControls(this.camera, this.targetCanvas);
      gsap.gsap.to(this.$refs.objectViewerDialogueWrapper, {
        visibility: "hidden",
        duration: 0.1,
        ease: "power4.out",
      });
      gsap.gsap.to(this.$refs.objectViewerDialogueContainer, {
        opacity: 0,
        scale: 0.5,
        duration: 0.5,
        ease: "power4.out",
      });
    },
  },
  mounted() {
    // Defining data variables
    let texture, normalMap, displacementMap;
    switch (this.objectData.objectType) {
      case "Cube":
        texture = new THREE.TextureLoader().load("/Block of diamond.png");
        normalMap = null;
        displacementMap = null;
        break;
      case "Sphere":
        texture = new THREE.TextureLoader().load("/rocks_ground_02_col_2k.png");
        normalMap = new THREE.TextureLoader().load(
          "/rocks_ground_02_nor_gl_2k.png"
        );
        displacementMap = new THREE.TextureLoader().load(
          "/rocks_ground_02_height_2k.png"
        );
        break;
      case "Pyramid":
        texture = new THREE.TextureLoader().load(
          "/castle_brick_02_red_diff_2k.png"
        );
        normalMap = new THREE.TextureLoader().load(
          "/castle_brick_02_red_nor_2k.png"
        );
        displacementMap = new THREE.TextureLoader().load(
          "/castle_brick_02_red_disp_2k.png"
        );
        break;
    }
    const grassTexture = new THREE.TextureLoader().load(
      "/brown_mud_leaves_01_diff_2k.jpg"
    );
    const grassBumperMap = new THREE.TextureLoader().load(
      "/brown_mud_leaves_01_bump_2k.png"
    );
    const grassDisplacementMap = new THREE.TextureLoader().load(
      "/brown_mud_leaves_01_disp_2k.png"
    );
    const grassNormalMap = new THREE.TextureLoader().load(
      "/brown_mud_leaves_01_nor_gl_2k.png"
    );
    this.targetCanvas = this.$refs.renderCanvas;
    this.scene = new THREE.Scene();
    const colors = {
      sceneColor: 0xf8f8f8,
    };
    // Creating geometries
    this.scene.background = new THREE.Color(colors.sceneColor);
    let megaObjectGeometry,
      minFilter = null;
    switch (this.objectData.objectType) {
      case "Cube":
        megaObjectGeometry = new THREE.BoxBufferGeometry(1, 1, 1, 512, 512);
        megaObjectGeometry.rotateY(Math.PI * 0.25);
        minFilter = THREE.NearestFilter;
        break;
      case "Sphere":
        megaObjectGeometry = new THREE.SphereBufferGeometry(0.5, 512, 512);
        break;
      case "Pyramid":
        megaObjectGeometry = new THREE.ConeBufferGeometry(0.5, 1, 512, 512);
        break;
    }

    // Creating mesh for the geometries
    const megaObjectMesh = new THREE.Mesh(
      megaObjectGeometry,
      new THREE.MeshStandardMaterial({
        color: 0xffffff,
        map: texture,
        normalMap: normalMap,
        displacementMap: displacementMap,
        displacementScale: 0.3,
        minFilter: minFilter,
        magFilter: minFilter,
      })
    );
    megaObjectMesh.castShadow = true;
    megaObjectMesh.receiveShadow = false;
    const megaPlaneMesh = new THREE.Mesh(
      new THREE.PlaneBufferGeometry(5, 5, 512, 512),
      new THREE.MeshStandardMaterial({
        color: 0xf0f0f0,
        map: grassTexture,
        normalMap: grassNormalMap,
        displacementMap: grassDisplacementMap,
        displacementScale: 0.05,
      })
    );
    // megaPlaneMesh.displacementBias = 0.01;
    megaPlaneMesh.receiveShadow = true;
    megaPlaneMesh.rotation.x = -Math.PI / 2;
    megaPlaneMesh.position.y = -0.6;

    // Creating camera for the scene
    this.camera = new THREE.PerspectiveCamera(
      50,
      this.targetCanvas.clientWidth / this.targetCanvas.clientHeight
    );
    this.camera.position.set(2, 2, 2);

    // Creating point light for the scene
    const directionalLight = new THREE.DirectionalLight(0xffffff, 1, 50);
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.4, 10);
    directionalLight.castShadow = true;
    directionalLight.position.y = 5;
    directionalLight.position.z = 6;
    directionalLight.shadow.mapSize.width = 512;
    directionalLight.shadow.mapSize.height = 512;
    directionalLight.shadow.camera.near = 0.5;
    directionalLight.shadow.camera.far = 50;
    directionalLight.shadow.camera.focus = 1;

    // Creating a renderer
    this.renderer = new THREE.WebGLRenderer({
      canvas: this.targetCanvas,
      antialias: true,
    });
    this.renderer.shadowMap.enabled = true;
    this.renderer.setClearColor(0x000000, 0);
    this.renderer.setSize(
      this.targetCanvas.clientWidth,
      this.targetCanvas.clientHeight
    );

    // Adding objects to the scene
    this.scene.add(directionalLight);
    this.scene.add(megaObjectMesh);
    this.scene.add(megaPlaneMesh);
    this.scene.add(this.camera);
    this.scene.add(ambientLight);

    // #region GUI
    const gui = new dat.GUI();
    gui.hide();

    // Color controls
    const colorControls = gui.addFolder("Color controls");
    colorControls.addColor(colors, "sceneColor").onChange(() => {
      this.scene.background = new THREE.Color(colors.sceneColor);
    });
    // Point light
    const directionalLightFolder = gui.addFolder("Point light");
    directionalLightFolder.add(directionalLight.position, "x", -5, 5, 0.1);
    directionalLightFolder.add(directionalLight.position, "y", -5, 5, 0.1);
    directionalLightFolder.add(directionalLight.position, "z", -5, 5, 0.1);
    // Light shadow mapping
    const directionalLightShadow = gui.addFolder("Light shadow");
    directionalLightShadow.add(directionalLight.shadow.mapSize, "width");
    directionalLightShadow.add(directionalLight.shadow.mapSize, "height");
    directionalLightShadow.add(directionalLight.shadow.camera, "near");
    directionalLightShadow.add(directionalLight.shadow.camera, "far");
    directionalLightShadow.add(directionalLight.shadow.camera, "focus");
    // Position
    const geometryPosition = gui.addFolder("Geometry position");
    geometryPosition.add(megaObjectMesh.position, "x", -5, 5, 0.1);
    geometryPosition.add(megaObjectMesh.position, "y", -5, 5, 0.1);
    geometryPosition.add(megaObjectMesh.position, "z", -5, 5, 0.1);
    // Camera position
    const cameraPosition = gui.addFolder("Camera position");
    cameraPosition.add(this.camera.position, "x", -5, 5, 0.1);
    cameraPosition.add(this.camera.position, "y", -5, 5, 0.1);
    cameraPosition.add(this.camera.position, "z", -5, 5, 0.1);
    // Camera Rotation
    const cameraRotation = gui.addFolder("Camera rotation");
    cameraRotation.add(this.camera.rotation, "x", -5, 5, 0.1);
    cameraRotation.add(this.camera.rotation, "y", -5, 5, 0.1);
    cameraRotation.add(this.camera.rotation, "z", -5, 5, 0.1);
    // Rotation
    const geometryRotation = gui.addFolder("Geometry rotation");
    geometryRotation.add(megaObjectMesh.rotation, "x", -5, 5, 0.1);
    geometryRotation.add(megaObjectMesh.rotation, "y", -5, 5, 0.1);
    geometryRotation.add(megaObjectMesh.rotation, "z", -5, 5, 0.1);
    // Plane position
    const planePosition = gui.addFolder("Plane position");
    planePosition.add(megaPlaneMesh.position, "x", -5, 5, 0.1);
    planePosition.add(megaPlaneMesh.position, "y", -5, 5, 0.1);
    planePosition.add(megaPlaneMesh.position, "z", -5, 5, 0.1);
    // Plane rotation
    const planeRotation = gui.addFolder("Plane rotation");
    planeRotation.add(megaPlaneMesh.rotation, "x", -5, 5, 0.1);
    planeRotation.add(megaPlaneMesh.rotation, "y", -5, 5, 0.1);
    planeRotation.add(megaPlaneMesh.rotation, "z", -5, 5, 0.1);
    // #endregion
    this.controls = new OrbitControls(this.camera, this.targetCanvas);

    // Updating renderer each frame per second
    this.tick();
  },
};
</script>

<style>
/* .object-item-wrapper {} */
.object-item-content {
  text-align: center;
  position: static;
  border: var(--border);
  overflow: hidden;
  border-radius: var(--border-radius);
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16);
  padding: 24px;
}
.object-item-content:hover {
  box-shadow: var(--object-item-drop-shadow);
}
.object-item-content::after {
  transition: opacity 0.5s cubic-bezier(0, 0, 0.58, 1);
  content: "";
  position: absolute;
  width: 400px;
  height: 10px;
  top: var(--shine-y);
  left: var(--shine-x);
  background: var(--dark-shine-background);
  opacity: 0;
}
.object-item-content:hover::after {
  opacity: 0.1;
}
.object-item-content > h2,
.object-viewer-dialogue-title-content > h2 {
  margin: 20px 0 30px;
  font-weight: 300;
  color: var(--text-color);
}
.object-item-content > p,
.object-viewer-dialogue-title-wrapper > p {
  color: var(--text-color);
  margin: 0;
}
.object-viewer-dialogue-title-wrapper > h2,
.object-viewer-dialogue-title-wrapper > p {
  text-align: left;
}
.object-viewer-dialogue-title-wrapper > h2 {
  font-weight: 400;
}
.object-item-canvas {
  border: var(--border);
  width: 100%;
  box-sizing: border-box;
  height: 300px;
  border-radius: var(--border-radius);
}
.object-viewer-dialogue-wrapper {
  visibility: hidden;
  z-index: 2;
  background-color: #ffffffa3;
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
}
.object-viewer-dialogue-container {
  color: var(--text-color);
  background-color: #fafafa;
  width: min-content;
  opacity: 0;
  transform: scale(0.5, 0.5);
  border-radius: var(--border-radius);
  padding: 24px;
  box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
}
.object-viewer-dialogue-canvas {
  width: 472px;
  height: 294px;
  border: var(--border);
  border-radius: var(--border-radius);
  margin: 24px auto;
}
.close-object-viewer-dialogue-button {
  background-color: transparent;
  float: right;
  transition: background-color 0.3s cubic-bezier(0.445, 0.05, 0.55, 0.95);
  cursor: pointer;
  border: none;
  font-family: Poppins;
  /* font-size: 14px; */
  border-radius: var(--border-radius);
  padding: 12px 24px;
  color: #ff762e;
}
.close-object-viewer-dialogue-button:hover {
  background-color: #f0f0f0;
}
</style>