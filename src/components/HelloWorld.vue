<template>
  <div class="hello">
    <canvas ref="cvs" :width="w" :height="h"></canvas>
  </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      w: 1000,
      h: 800,
      camera: null,
      cube: null,
      renderer: null,
      control: null
    }
  },
  mounted() {
    this.initWebGl()
    this.animate()
  },
  methods: {
    initWebGl() {
      const canvas = this.$refs.cvs

      // 渲染场景
      this.scene = new THREE.Scene();

      // 渲染的相机
      this.camera = new THREE.PerspectiveCamera( 75, this.w / this.h, 0.1, 1000 );
      this.camera.position.x = 0
      this.camera.position.y = 2
      this.camera.position.z = 10

      // 添加日光
      this.scene.add(new THREE.AmbientLight(0x404040))

      // 坐标轴
      const axesHelper = new THREE.AxesHelper( 5 );
      this.scene.add( axesHelper );

      // 渲染器添加
      this.renderer = new THREE.WebGLRenderer({canvas, antialias: true, alpha: false});
      this.renderer.setSize( this.w, this.h );

      // 添加几何体
      const geometry = new THREE.BoxGeometry( 2, 2, 2 );
      const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
      this.cube = new THREE.Mesh( geometry, material );
      this.cube.position.x = 0
      this.cube.position.y = 0
      this.cube.position.z = 0
      const dodecahedronGeometry = new THREE.DodecahedronGeometry(2,1)
      const dodeMaterial = new THREE.MeshBasicMaterial( { color: 0x0000ff } );
      const dodeMesh = new THREE.Mesh(dodecahedronGeometry, dodeMaterial)
      dodeMesh.position.y = 4
      this.scene.add( this.cube );
      this.scene.add( dodeMesh );

      // 添加控制器
      this.controls = new OrbitControls(this.camera, this.renderer.domElement)
    },
    animate() {
      requestAnimationFrame( this.animate );
      // this.cube.rotation.x += 0.01;
      // this.cube.rotation.y += 0.01;
      // 在渲染器中渲染出三维场景
      this.renderer.render( this.scene, this.camera );
      this.controls.update()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
