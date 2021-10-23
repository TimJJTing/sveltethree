<script>
  import Scene from "./Scene.svelte";
  let color = 0x0000ff;
  let light = true;
  let tubeRotation = true;
  let lightRotation = false;
  const getRandomColor = () => {
    const r = Math.floor(Math.random() * 0xff) << 16;
    const g = Math.floor(Math.random() * 0xff) << 8;
    const b = Math.floor(Math.random() * 0xff);
    return r + g + b;
  };
</script>

<div class="container">
  <Scene
    {color}
    usePointLight={light}
    rotateCube={tubeRotation}
    rotateLight={lightRotation}
  />
  <div class="tools">
    <button on:click={() => (color = getRandomColor())}> Change Color </button>
    <button on:click={() => (light = !light)}> Light </button>
    <button on:click={() => (tubeRotation = !tubeRotation)}>
      Tube Rotation
    </button>
    <button on:click={() => (lightRotation = !lightRotation)}>
      Light Rotation
    </button>
  </div>
</div>

<style>
  div.container {
    position: relative;
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
  }
  div.tools {
    position: absolute;
    top: 0;
    left: 0;
    margin: 10px;
    width: 100%;
    height: fit-content;
    max-height: 60px;
    box-sizing: border-box;
    z-index: 1;
  }
</style>
