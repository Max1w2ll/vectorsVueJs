<template>
  <div class="mainWindow">
    <div class="header"></div>

    <div class="leftSidePanel"> 
      <div class="header">
          <div class="titleAndImage">
            <img class="titleIcon" src="@/assets/pen.png">
            <span class="leftSideTitle"> Решение </span>
          </div>
      </div>
    </div>


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
            <div class="coordinatesBegin">
              <input>
              <input>
              <input>
            </div>
            <div class="coordinatesEnd">
              <input>
              <input>
              <input>
            </div>
          </div>

          <div style="margin-top: 40px;" class="headerCoordinates">
            <span> X </span>
            <span> Y </span>
            <span> Y </span>
          </div>

          <div class="coordinateInputs">
            <div class="coordinatesBegin">
              <input>
              <input>
              <input>
            </div>
            <div class="coordinatesEnd">
              <input>
              <input>
              <input>
            </div>
          </div>
        </div>

      </div>

      <div class="taskButtons">
        <button> Сумма двух векторов </button> 
        <button> Разность двух векторов </button> 
        <button> Скалярное произведение двух векторов </button> 
        <button> Угол между двумя векторами </button>  
      </div>

      <div class="executeOrClearButtons">
        <button class="executeButton"> Выполнить </button>
        <button class="clearButton"> Очистить </button>
      </div>

    </div>


  </div>
</template>

<script>
export default {
  name: 'Main',
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

  /* Left side panel */

  .leftSidePanel {
    height: 100%;
    width: 250px;

    background: var(--panel-main-background);
  }

  .leftSidePanel .header {
    margin-top: 2px;
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
