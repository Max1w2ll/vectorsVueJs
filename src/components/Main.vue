<template>
  <div id="mainWindow" class="mainWindow">
    <div class="header"></div>

    <div class="leftSidePanel"> 
      <div class="header">
          <div class="titleAndImage">
            <img class="titleIcon" src="@/assets/pen.png">
            <span class="leftSideTitle"> Решение </span>
          </div>
      </div>
    </div>

    <div id="sceneHolder" class="sceneHolder"></div>

    <div class="rightSidePanel"> 
      <div class="header">
        <div class="titleAndImage">
          <img class="titleIcon" src="@/assets/coordinates.png">
          <span class="leftSideTitle"> Векторы </span>
        </div>
      </div>

      <div class="parametersSection">
        <div class="parametersHeader">
          <span> Параметры осей </span>
        </div> 
        
        <div class="parametersVector">
          <div class="headerCoordinates">
            <span> X </span>
            <span> Y </span>
            <span> Z </span>
          </div>

          <div class="coordinateInputs">
            <form id="firstFormCheckCoordsBegin" class="coordinatesBegin">
              <input minlength="1" required="true" type="number" id="X1-B" placeholder="X1-B">
              <input minlength="1" required="true" type="number" id="Y1-B" placeholder="Y1-B">
              <input minlength="1" required="true" type="number" id="Z1-B" placeholder="Z1-B">
            </form>
            <form id="firstFormCheckCoordsEnd" class="coordinatesEnd">
              <input minlength="1" required="true" type="number" id="X1-E" placeholder="X1-E">
              <input minlength="1" required="true" type="number" id="Y1-E" placeholder="Y1-E">
              <input minlength="1" required="true" type="number" id="Z1-E" placeholder="Z1-E">
            </form>
          </div>

          <div style="margin-top: 40px;" class="headerCoordinates">
            <span> X </span>
            <span> Y </span>
            <span> Y </span>
          </div>

          <div class="coordinateInputs">
            <form id="secFormCheckCoordsBegin" class="coordinatesBegin">
              <input minlength="1" required="true" type="number" id="X2-B" placeholder="X2-B">
              <input minlength="1" required="true" type="number" id="Y2-B" placeholder="Y2-B">
              <input minlength="1" required="true" type="number" id="Z2-B" placeholder="Z2-B">
            </form>
            <form id="secFormCheckCoordsEnd" class="coordinatesEnd">
              <input minlength="1" required="true" type="number" id="X2-E" placeholder="X2-E">
              <input minlength="1" required="true" type="number" id="Y2-E" placeholder="Y2-E">
              <input minlength="1" required="true" type="number" id="Z2-E" placeholder="Z2-E">
            </form>
          </div>
        </div>

      </div>

      <div class="taskButtons">
        <button @click="SumVectors()"> Сумма двух векторов </button> 
        <button @click="DiffVectors()"> Разность двух векторов </button> 
        <button @click="ScalMultVectors()"> Скалярное произведение двух векторов </button> 
        <button @click="DegreeVectors()"> Угол между двумя векторами </button>  
      </div>

      <div class="executeOrClearButtons">
        <button class="executeButton"> Выполнить </button>
        <button class="clearButton"> Очистить </button>
      </div>

    </div>


  </div>
</template>

<script>
import * as THREE from 'three';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
const renderer = new THREE.WebGLRenderer();
const controls = new OrbitControls( camera, renderer.domElement );

export default {
  name: 'Main',

  data() {
    return {

    }
  },

  mounted() {
    this.setScene();
    this.createAxis();
    this.createAxisText();
    this.setCameraPosition();
  },

  methods: {
    setScene() {
      // Set sizes and place it in sceneHolder
      renderer.setSize( window.innerWidth, window.innerHeight );
      const sceneHolder = document.querySelector('#sceneHolder');
      sceneHolder.appendChild( renderer.domElement );

      function animate() {
        requestAnimationFrame( animate );

        // required if controls.enableDamping or controls.autoRotate are set to true
	      controls.update();

        renderer.render( scene, camera );

      }
      animate();
    },

    setCameraPosition() {
      camera.position.z = 30;
      camera.position.x = 30;
      camera.position.y = 10;

      camera.rotation.y = 0.8
    },

    createAxis() {
      // Creating XYZ axis
      this.createLine(0xff0000, [-10000, 0, 0], [10000, 0, 0], true)
      this.createLine(0x00ff00, [0, -10000, 0], [0, 10000, 0], true)
      this.createLine(0x0000ff, [0, 0, -10000], [0, 0, 10000], true)
    },

    createAxisText() {
      this.createText('#ff0000', "X", [6, 1, 0], [0, 0, 0]);
      this.createText('#00ff00', "Y", [4, 4, 0],  [0, 0, 0]);
      this.createText('#0000ff', "Z", [0, 1, 2],  [0, 1.7, 0]);
    },

    createText(color, text, coords, rotation) {
      const canvas = document.createElement('canvas')
      const context = canvas.getContext('2d')

      context.fillStyle = color
      context.font = '60px sans-serif'
      context.fillText(text, 10, 60, 10000)

      // canvas contents are used for a texture
      const texture = new THREE.Texture(canvas)
      texture.needsUpdate = true

      var material = new THREE.MeshBasicMaterial({
        map: texture,
        side: THREE.DoubleSide,
      })
      material.transparent = true
      var mesh = new THREE.Mesh(new THREE.PlaneGeometry(5, 5), material)

      mesh.position.x = coords[0];
      mesh.position.y = coords[1];
      mesh.position.z = coords[2];

      mesh.rotation.x = rotation[0];
      mesh.rotation.y = rotation[1];
      mesh.rotation.z = rotation[2];

      scene.add(mesh);
    },

    createLine(color, coordBegin, coordEnd, dashed) {
      if (dashed) { // For axis mostly
        var material = new THREE.LineDashedMaterial( {
          color: color,
          linewidth: 1,
          scale: 1,
          dashSize: 1,
          gapSize: 0.5,
        });
      }
      else {
        var material =  new THREE.LineBasicMaterial( { color: color } );
      }

      const points = [];
      points.push( new THREE.Vector3( coordBegin[0], coordBegin[1], coordBegin[2] ));
      points.push( new THREE.Vector3( coordEnd[0], coordEnd[1], coordEnd[2] ));

      const geometry = new THREE.BufferGeometry().setFromPoints( points );
      const line = new THREE.Line( geometry, material );
      line.computeLineDistances();

      scene.add( line );
    },

    SumVectors() {
      this.getUserCoords();
    },

    getUserCoords() {
      const firstFormCheckCoordsBegin = document.getElementById('firstFormCheckCoordsBegin').checkValidity();
      const firstFormCheckCoordsEnd = document.getElementById('firstFormCheckCoordsEnd').checkValidity();

      const secFormCheckCoordsBegin = document.getElementById('secFormCheckCoordsBegin').checkValidity();
      const secFormCheckCoordsEnd = document.getElementById('secFormCheckCoordsBegin').checkValidity();

      if (firstFormCheckCoordsBegin && firstFormCheckCoordsEnd && secFormCheckCoordsBegin && secFormCheckCoordsEnd) {
        const firstLineCoordsBegin = [document.getElementById('X1-B').value, document.getElementById('Y1-B').value, document.getElementById('Z1-B').value];
        const firstLineCoordsEnd = [document.getElementById('X1-E').value, document.getElementById('Y1-E').value, document.getElementById('Z1-E').value];

        const secLineCoordsBegin = [document.getElementById('X2-B').value, document.getElementById('Y2-B').value, document.getElementById('Z2-B').value];
        const secLineCoordsEnd = [document.getElementById('X2-E').value, document.getElementById('Y2-E').value, document.getElementById('Z2-E').value];

        console.log(firstLineCoordsBegin);
        console.log(firstLineCoordsEnd);

        console.log(secLineCoordsBegin);
        console.log(secLineCoordsEnd);
      }
    }
  }
}

</script>

<style>
  /* Fonts and colors */

  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300&display=swap');

  :root {
    --main-background: #252628;
    --panel-header-background: #424848;
    --panel-main-background: #373c3d;

    --inputs-background-hover: #616768;
    --inputs-background-hover-blue: #12b5f5;
    --inputs-borders-color: #5c5e68;

    --main-color: #009cd8;
    --parameters-color: #e1e3ee;

    --main-font: 'Roboto', sans-serif;
  }

  /* Main stuff */
  
  body {
    top: 0;
    left: 0;

    margin: 0;

    overflow-y: hidden;
    overflow-x: hidden;

    font-family: var(--main-font);
  }

  .mainWindow {
    height: 1080px;
    width: 1920px;

    background: var(--main-background);
  }

  .header {
    height: 40px;
    width: 100%;

    background: var(--panel-header-background);
  }

  /* No buttons on input number on all browsers */
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  input[type=number] {
    -moz-appearance: textfield;
  }

  /* Left side panel */

  .leftSidePanel {
    top: 42px;
    left: 0;

    position: absolute;

    height: 100%;
    width: 250px;

    background: var(--panel-main-background);
  }

  .titleAndImage {
    padding-top: 7px;
    padding-left: 10px;

    font-size: 18px;

    color: var(--main-color);
  }

  .titleIcon {
    transform: translateY(3px);

    margin-right: 5px;

    height: 20px;
    width: 20px;

    filter: invert(49%) sepia(72%) saturate(5010%) hue-rotate(171deg) brightness(100%) contrast(104%);
  }

  /* Main section */

  .sceneHolder {
    background: #009cd8;
  }


  /* Right side panel */

  .rightSidePanel {
    right: 0px;
    top: 42px;

    position: absolute;

    width: 300px;
    height: 650px;

    background: var(--panel-main-background);
  }

  .parametersSection {
    height: 270px;

    color: var(--parameters-color);
  }

  .parametersHeader {
    margin: 20px;
    text-align: center;
  }

  .parametersVector {
    height: 150px;
  }

  .headerCoordinates {
    margin-top: 20px;

    text-align: center;
  }

  .headerCoordinates span {
    display: inline-block;

    width: 72px;
  }

  .coordinateInputs {
    margin-top: 10px;

    display: grid;

    justify-content: center;
  }

  .coordinateInputs input {
    height: 20px;
    width: 65px;

    border: solid var(--inputs-borders-color) 1px;

    color: var(--parameters-color);
    background: transparent;
  }

  .coordinateInputs input::placeholder {
    text-align: center;
  }

  .taskButtons {
    margin: auto;

    display: grid;
    width: fit-content;

    text-align: center;
  }

  .taskButtons button {
    all: unset;

    margin-bottom: 5px;

    padding: 5px;

    height: 32px;
    width: 200px;

    border: 1px solid var(--inputs-borders-color);

    color: var(--parameters-color);
    
    font-size: 14px;

    transition: 0.2s;
    cursor: pointer;
  }
  .taskButtons button:hover {
    background: var(--inputs-background-hover);
  }

  .executeOrClearButtons {
    margin: auto;
    margin-top: 20px;

    width: fit-content;

    text-align: center;

    display: grid;
  }

  .executeOrClearButtons button {
    margin-bottom: 5px;

    height: 35px;
    width: 210px;

    font-size: 14px;

    color: var(--parameters-color);

    transition: 0.2s;

    cursor: pointer;
  }

  .executeButton {
    all: unset;

    background: var(--main-color);
  }

  .executeButton:hover {
    background: var(--inputs-background-hover-blue);
  }

  .clearButton {
    all: unset;

    border: 1px solid var(--inputs-borders-color);
  }

  .clearButton:hover {
    background: var(--inputs-background-hover);
  }

</style>
