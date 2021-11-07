<template>
  <div>
    <axis3d-comp>
      <canvas class="axis3dComp_canvas" />
    </axis3d-comp>
  </div>
</template>

<script>
  import Axis3dComp from './Axis3dComp';

  export default {
    name: 'App',
    components: {
      Axis3dComp
    }
  };
</script>

<style>
  .axis3dComp_canvas {
    width: 1000px;
    height: 700px;
  }
</style>