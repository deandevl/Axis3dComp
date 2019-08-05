<template>
  <div>
    <slot></slot>
  </div>
</template>

<script>
  //import BABYLON from 'babylonjs';

  export default {
    name: 'Axis3dComp',
    data(){
      return {
        canvas: null,
        engine: null,
        scene: null,
        axis_size: 8
      }
    },
    methods: {
      create_scene: function(){
        const scene = new BABYLON.Scene(this.engine);
        //camera
        //const camera = new BABYLON.ArcRotateCamera("camera_1",0,0,0,new BABYLON.Vector3(5,3,0),scene);
        const camera = new BABYLON.ArcRotateCamera("camera_1",0,0,0,new BABYLON.Vector3.Zero(),scene);
        //camera.setPosition(new BABYLON.Vector3(10.,6.,-10.));
        camera.setPosition(new BABYLON.Vector3(0.,0.,-32.));
        camera.attachControl(this.canvas,true);
        //lights
        const light = new BABYLON.HemisphericLight("light_1",new BABYLON.Vector3(1,.5,0),scene);
        light.intensity = 0.8;
        //material
        const mat = new BABYLON.StandardMaterial("mat_1",scene);
        mat.alpha = 1.0;
        mat.diffuseColor = new BABYLON.Color3(0.5,0.5,1.0);
        mat.backFaceCulling = false;

        //create axises
        const size = this.axis_size;

        const x = BABYLON.Mesh.CreateLines("x",[
          new BABYLON.Vector3.Zero(),
          new BABYLON.Vector3(size,0,0),
          new BABYLON.Vector3(0.95*size,0.05*size,0),
          new BABYLON.Vector3(size,0,0),
          new BABYLON.Vector3(0.95*size,-0.05*size,0),
        ],scene);
        x.color = new BABYLON.Color3(1,0,0);
        const x_char = this.makeTextPlane("X","red",size/10);
        x_char.position = new BABYLON.Vector3(0.9*size,-0.05*size,0);

        const y = BABYLON.Mesh.CreateLines("y",[
          new BABYLON.Vector3.Zero(),
          new BABYLON.Vector3(0,size,0),
          new BABYLON.Vector3(-.05*size,0.95*size,0),
          new BABYLON.Vector3(0,size,0),
          new BABYLON.Vector3(0.05*size,0.95*size,0)
        ],scene);
        y.color = new BABYLON.Color3(0,1,0);
        const y_char = this.makeTextPlane("Y","green",size/10);
        y_char.position = new BABYLON.Vector3(0,0.9*size,-0.05*size);

        const z = BABYLON.Mesh.CreateLines("z",[
          new BABYLON.Vector3.Zero(),
          new BABYLON.Vector3(0,0,size),
          new BABYLON.Vector3(0,-0.05*size,0.95*size),
          new BABYLON.Vector3(0,0,size),
          new BABYLON.Vector3(0,0.05*size,0.95*size)
        ],scene);
        z.color = new BABYLON.Color3(0,0,1);
        const z_char = this.makeTextPlane("Z","blue",size/10);
        z_char.position = new BABYLON.Vector3(0,0.05*size,0.9*size);

        //add some vectors
        const v_x1 = 4./3.*size*.25;
        const v_x2 = 0.;
        const v_x3 = size*.25;

        const vec = BABYLON.Mesh.CreateLines("vec",[
          new BABYLON.Vector3.Zero(),
          //new BABYLON.Vector3(-v_x2,-v_x3,-v_x1),
          new BABYLON.Vector3(v_x1,v_x2,v_x3)
        ],scene);
        vec.color = new BABYLON.Color3(1,1,0);

        const vec1 = BABYLON.Mesh.CreateLines("vec1",[
          new BABYLON.Vector3(v_x1,0,0),
          new BABYLON.Vector3(v_x1,0,0.05*size)
        ],scene);
        vec1.color = new BABYLON.Color3(1,1,0);

        const vec2 = BABYLON.Mesh.CreateLines("vec2",[
          new BABYLON.Vector3(0,0,v_x3),
          new BABYLON.Vector3(0.05*size,0,v_x3)
        ],scene);
        vec2.color = new BABYLON.Color3(1,1,0);

        /*
              const plane = BABYLON.Plane.FromPoints(
                new BABYLON.Vector3(0,0,0),
                new BABYLON.Vector3(.3,1,0),
                new BABYLON.Vector3(.2,0,1)
              );
              */
        return scene;
      },
      makeTextPlane: function(text,color,size){
        const scene = this.scene;
        const dynamicTexture = new BABYLON.DynamicTexture("DynamicTexture",50,scene,true);
        dynamicTexture.hasAlpha = true;
        dynamicTexture.drawText(text,5,40,"bold 36px Arial",color,"transparent",true);
        const plane = new BABYLON.Mesh.CreatePlane("TextPlane",size,scene,true);
        plane.material = new BABYLON.StandardMaterial("TextPlaneMaterial",scene);
        plane.material.backFaceCulling = false;
        plane.material.specularColor = new BABYLON.Color3(0,0,0);
        plane.material.diffuseTexture = dynamicTexture;
        return plane;
      }
    },
    mounted(){
      this.canvas = this.$slots.default[0].elm;
      this.engine = new BABYLON.Engine(this.canvas,true);
      this.scene = this.create_scene();
      this.engine.runRenderLoop(() => {
        this.scene.render();
      });
    }
  }
</script>
<style>
</style>