<template>
  <div class="main">
    <div id="screen" class="screen"></div>
  </div>
</template>

<script>
import ZThree from '@/three/ZThree.js'
import * as THREE from 'three'
import { loaderModel } from '@/three/loaderModel.js'

let app, camera, scene, renderer, clock

export default {
  name: 'HomeView',
  methods: {
    async initZThree() {
      // 初始化
      app = await new ZThree('screen')
      app.initThree()
      app.initOrbitControls()
      app.initLight()

      window.app = app
      camera = app.camera
      scene = app.scene
      renderer = app.renderer
      clock = new THREE.Clock()

      // 相机位置
      camera.position.set(30, 30, 30)
      camera.lookAt(0, 0, 0)

      // 加载模型
      await loaderModel(app)

      // 渲染循环
      app.render(() => {
        const delta = clock.getDelta()
        renderer.render(scene, camera)
      })
    }
  },
  mounted() {
    this.initZThree()
  }
}
</script>

<style lang="less">
.main {
  width: 100%;
  height: 100%;
  overflow: hidden;
  background: #000;
}
.screen {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
}
</style>
