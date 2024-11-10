<template>
    <div id="threeContainer">
      <canvas class="box1" ref="box1"></canvas>
      <canvas class="box2" ref="box2"></canvas>
    </div>
  </template>
  
  <script setup>
  import { onMounted, ref } from 'vue';
  import * as THREE from 'three';
  import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';
  
  const box1 = ref(null);
  const box2 = ref(null);

  
  onMounted(() => {
  console.log(box1.value,'ssss111------------------------')

    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer({
      canvas: box1.value
  });
    scene.background = new THREE.Color(); // 设置背景为浅蓝色
    // renderer.setSize(window.innerWidth, window.innerHeight);
    // threeContainer.value.appendChild(renderer.domElement);
  
    // 设置相机位置
    camera.position.z = 5;
    camera.lookAt(0, 0, 0);

//路
const roadMaterial = new THREE.LineBasicMaterial({ color: 0x000000 }); // 黑色线条




// 灯光
const directionalLight = new THREE.DirectionalLight(0xffffff, 0.5);
directionalLight.position.set(0, 20, 10);
scene.add(directionalLight);
// const ambientLight = new THREE.AmbientLight(0x404040); // 软白色光
// scene.add(ambientLight);
    const spotLight = new THREE.SpotLight(0xffffff, 20);
spotLight.position.set(0, 10, 10);

spotLight.decay = 0;
spotLight.castShadow = true;
spotLight.shadow.mapSize.width = 1024;
spotLight.shadow.mapSize.height = 1024;
scene.add(spotLight);

// 创建一个GLTFLoader实例
const loader = new GLTFLoader();
    let gltfScene = null; // 用于存储加载的模型


    // 加载外部模型
    loader.load(
      'pony_cartoon2.glb', // 模型文件的路径
      (gltf) => {
        // 加载完成后将模型添加到场景中
        gltfScene = gltf.scene;
        gltfScene.rotation.set(0, Math.PI / 2, 0);
        scene.add(gltf.scene);

      },
      (progress) => {
        console.log('加载进度：', progress);
      },
      (error) => {
        console.error('加载错误：', error);
      }
    );

    const path = [
  new THREE.Vector3(0, 0, 0),
  new THREE.Vector3(10, 0, 0),
  new THREE.Vector3(10, 0, 0),
  new THREE.Vector3(10, 0, 0),
  new THREE.Vector3(0, 0, 0)
];
let currentPoint = 0;
let angle = 0
const speed = 0.01; // 控制小汽车的速度
    // 渲染循环
    const animate = () => {
      requestAnimationFrame(animate);
      // 如果模型已加载，更新模型的旋转
    // if (gltfScene) {
    //   const t = Math.min((Date.now() % 10000) / 10000, 1); // 循环插值
    // const pointA = path[currentPoint];
    // const pointB = path[(currentPoint + 1) % path.length];
    // gltfScene.position.lerpVectors(pointA, pointB, t);

    // // 根据速度更新当前点
    // if (t >= 1) {
    //   currentPoint = (currentPoint + 1) % path.length;
    // }
    // }
    // gltfScene.rotation.x += 0.01;
    // gltfScene.rotation.y += 0.01;

    const radius = 3
    angle += 0.01
    camera.position.x = radius * Math.sin(angle)
    camera.position.z = radius * Math.cos(angle)
    camera.lookAt(0, 0, 0)

    
      renderer.render(scene, camera);
    };
  
    animate();
  });
  
  // 监听窗口大小变化
//   window.addEventListener('resize', onWindowResize, false);
  
//   function onWindowResize() {
//     const width = window.innerWidth;
//     const height = window.innerHeight;
//     camera.aspect = width / height;
//     camera.updateProjectionMatrix();
//     renderer.setSize(width, height);
//   }
  </script>
  
  <style scoped>
  #threeContainer {
    height: 100vh;
    display: flex;
    justify-content: center;
  }
  .box1,.box2{
    width: 500px;
    height: 500px;
    border: 1px solid black;
    margin: 0 20px
  }

  </style>