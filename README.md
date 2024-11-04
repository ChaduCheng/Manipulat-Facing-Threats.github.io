
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="description"
        content="Mamba Policy">
  <meta name="keywords" content="Manipulation, 3D Vision, Mamba">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Mamba Policy</title>

  <script>
    // functions borrowed from PaLM-E
    timeoutIds = [];
        
    function populateDemo(img) {
      console.log("img", img)

      var img1 = document.getElementById("scene1");
      img1.onclick = null;

      var img2 = document.getElementById("scene2");
      img2.onclick = null;

      var img3 = document.getElementById("scene3");
      img3.onclick = null;

      var img4 = document.getElementById("scene4");
      img4.onclick = null;

      var img5 = document.getElementById("scene5");
      img5.onclick = null;

      var img6 = document.getElementById("scene6");
      img6.onclick = null;

      for (const idx of [1,2,3,4,5]) {
        var instruction = document.getElementById("instruction"+idx);
        var response = document.getElementById("response"+idx);
        instruction.innerHTML = "";
        response.innerHTML = "";
      }

      model1 = scene1.getObjectByName("mesh")
      scene1.remove(model1)
      document.querySelector('#pose_loading').innerHTML = `<img src="assets/loading.svg" width="48" height="48">`

      var capability = document.querySelector('input[name="capability"]:checked').value;
      let assetUrl = new URL('./assets/scene_mesh/' + capability + img.id[5] + '.glb', document.URL)
      assetLoader1.load(assetUrl.href, gltf => {
        model1 = gltf.scene
        model1.name = "mesh"
        scene1.add(model1)
        document.querySelector('#pose_loading').innerHTML = ''

        img1.onclick = function() {populateDemo(img1)};
        img2.onclick = function() {populateDemo(img2)};
        img3.onclick = function() {populateDemo(img3)};
        img4.onclick = function() {populateDemo(img4)};
        img5.onclick = function() {populateDemo(img5)};
        img6.onclick = function() {populateDemo(img6)};

        var qas = img.alt.split("[SEP]");
        for (timeoutId of timeoutIds) {
            clearTimeout(timeoutId);
        }
        var delay = 0;
        for ([idx, qa] of qas.entries()) {
          idx += 1;
          qa = qa.split("[sep]")
          timeoutIds.push(setTimeout(displayDialogue, delay, qa, idx));
          delay += qa[1].length * 25;
        }
      }, undefined, (error) => {console.error(error)})
    }

    function typeWriter(txt, i, q, idx) {
      var instruction = document.getElementById("instruction"+idx);
      if (instruction.innerHTML == q) {
        if (i < txt.length) {
          document.getElementById("response"+idx).innerHTML += txt.charAt(i);
          i++;
          timeoutIds.push(setTimeout(typeWriter, 20, txt, i, q, idx));
        }
      }
    }

    function displayDialogue(qa, idx) {
      var instruction = document.getElementById("instruction"+idx);
      var response = document.getElementById("response"+idx);
      instruction.innerHTML = qa[0];
      response.innerHTML = "";
      typeWriter(qa[1], 0, qa[0], idx);
    }

  </script>

  <script type="importmap">
    {
      "imports": {
        "vue": "https://unpkg.com/vue@3/dist/vue.esm-browser.js",
        "three": "https://unpkg.com/three@0.127.0/build/three.module.js",
        "three/addons/": "https://unpkg.com/three@0.127.0/examples/jsm/"
      }
    }
  </script>

  <!-- imported in PaLM-E -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.4.0/css/font-awesome.min.css">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.8.0/codemirror.min.css">
  <link rel="stylesheet" href="https://github.com/palm-e/palm-e.github.io/blob/main/css/app.css">

  <link rel="stylesheet" href="https://github.com/palm-e/palm-e.github.io/blob/main/css/bootstrap.min.css">

  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.8.0/codemirror.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/clipboard.js/1.5.3/clipboard.min.js"></script>
  
  <script src="https://github.com/palm-e/palm-e.github.io/blob/main/js/app.js"></script>

  <!-- imported in Nerfies -->
  <link href="https://fonts.googleapis.com/css?family=Google+Sans|Noto+Sans|Castoro"
        rel="stylesheet">

  <link rel="stylesheet" href="./static/css/bulma.min.css">
  <link rel="stylesheet" href="./static/css/bulma-carousel.min.css">
  <link rel="stylesheet" href="./static/css/bulma-slider.min.css">
  <link rel="stylesheet" href="./static/css/fontawesome.all.min.css">
  <link rel="stylesheet"
        href="https://cdn.jsdelivr.net/gh/jpswalsh/academicons@1/css/academicons.min.css">
  <link rel="stylesheet" href="./static/css/index.css">
  <link rel="icon" href="./assets/mamba-policy-logo.svg">

  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
  <script defer src="./static/js/fontawesome.all.min.js"></script>
  <script src="./static/js/bulma-carousel.min.js"></script>
  <script src="./static/js/bulma-slider.min.js"></script>
  <script src="https://kit.fontawesome.com/69dc91e44b.js" crossorigin="anonymous"></script>

</head>

<body>

<!-- <nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false">
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
    </a>
  </div>
  <div class="navbar-menu">
    <div class="navbar-start" style="flex-grow: 1; justify-content: center;">
      <a class="navbar-item" target="_blank" href="https://siyuanhuang.com/">
        <span class="icon">
          <i class="fas fa-home"></i>
        </span>
      </a>

      <div class="navbar-item has-dropdown is-hoverable">
        <a class="navbar-link">
        More Research
        </a>
        <div class="navbar-dropdown">
        <a class="navbar-item" target="_blank" href="https://scenediffuser.github.io/">
            SceneDiffuser
        </a>
        <a class="navbar-item" target="_blank" href="https://sqa3d.github.io/">
            SQA3D
        </a>
        <a class="navbar-item" target="_blank" href="https://arnold-benchmark.github.io/">
            ARNOLD
        </a>
        <a class="navbar-item" target="_blank" href="https://3d-vista.github.io/">
            3D-VisTA
        </a>
        </div>
      </div>
    </div>
  </div>
</nav>
 -->
<section class="hero">
  <div class="hero-body">
    <div class="container is-max-desktop">
      <div class="columns is-centered">
        <div class="column has-text-centered">
          <img src="assets/mamba-policy-logo.svg" width="10%">
          <h1 class="title is-1 publication-title">Mamba Policy: Towards Efficient 3D Diffusion Policy with<br>Hybrid Selective State Models</h1>
<!--           <h4 class="title is-4 publication-title">Under Review</h4> -->
          <div class="is-size-5 publication-authors">
            <span class="author-block">
              <a target="_blank" href="https://andycao1125.github.io/mamba_policy/">Jiahang Cao</a><sup>1✶</sup>,</span>
            <span class="author-block">
              <a target="_blank" href="https://andycao1125.github.io/mamba_policy/">Qiang Zhang</a><sup>1✶</sup>,</span>
            <span class="author-block">
              <a target="_blank" href="https://andycao1125.github.io/mamba_policy/">Jingkai Sun</a><sup>1</sup>,</span>
            <span class="author-block">
              <a target="_blank" href="https://andycao1125.github.io/mamba_policy/">Jiaxu Wang</a><sup>1</sup>,</span>
            <span class="author-block">
              <a target="_blank" href="https://andycao1125.github.io/mamba_policy/">Hao Cheng</a><sup>1</sup>,</span>
            <span class="author-block">
              <a target="_blank" href="https://andycao1125.github.io/mamba_policy/">Yulin Li</a><sup>2</sup>,</span>
            <br/>
            <span class="author-block">
              <a target="_blank" href="https://andycao1125.github.io/mamba_policy/">Jun Ma</a><sup>2</sup>,</span>
            <span class="author-block">
              <a target="_blank" href="https://andycao1125.github.io/mamba_policy/">Yecheng Shao</a><sup>4</sup>,</span>
            <span class="author-block">
              <a target="_blank" href="https://andycao1125.github.io/mamba_policy/">Wen Zhao</a><sup>3</sup>,</span>
            <span class="author-block">
              <a target="_blank" href="https://andycao1125.github.io/mamba_policy/">Gang Han</a><sup>3</sup>,</span>
            <span class="author-block">
              <a target="_blank" href="https://andycao1125.github.io/mamba_policy/">Yijie Guo</a><sup>3</sup>,</span>
            <span class="author-block">
              <a target="_blank" href="https://andycao1125.github.io/mamba_policy/">Renjing Xu</a><sup>1†</sup></span>
          </div>

          <div class="is-size-5 publication-authors">
            <span class="author-block"><sup>1</sup>The Hong Kong University of Science and Technology (Guangzhou)</span>
            <br/>
            <span class="author-block"><sup>2</sup>The Hong Kong University of Science and Technology</span>
            <br/>
            <span class="author-block"><sup>3</sup>Beijing Innovation Center of Humanoid Robotics</span>
            <span class="author-block"><sup>4</sup>Center for X-Mechanics, Zhejiang University</span>
          </div>

          <p style="font-size: 0.9em; padding: 0.5em 0 0 0;">✶ indicates equal contribution</p>

          <div class="column has-text-centered">
            <div class="publication-links">
              <!-- Arxiv Link. -->
              <span class="link-block">
                <a target="_blank" href="https://www.arxiv.org/abs/2409.07163"
                   class="external-link button is-normal is-rounded is-dark">
                  <span class="icon">
                      <i class="ai ai-arxiv"></i>
                  </span>
                  <span>arXiv</span>
                </a>
              </span>
              <!-- Video Link. -->
              <span class="link-block">
                <a target="_blank" href="https://andycao1125.github.io/mamba_policy"
                   class="external-link button is-normal is-rounded is-dark">
                  <span class="icon">
                      <i class="fab fa-youtube"></i>
                  </span>
                  <span>Video</span>
                </a>
              </span>
              </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<section class="hero teaser">
  <div class="container is-max-desktop">

    <!-- Abstract. -->
    <div class="columns is-centered has-text-centered">
      <div class="column is-four-fifths">
        <h2 class="title is-3">Abstract</h2>
        <div class="content has-text-justified">
          <p>
            Diffusion models have been widely employed in the field of 3D manipulation due to their efficient capability to learn distributions, allowing for precise prediction of action trajectories. 
            However, diffusion models typically rely on large parameter UNet backbones as policy networks, which can be challenging to deploy on resource-constrained devices. 
            Recently, the Mamba model has emerged as a promising solution for efficient modeling, offering low computational complexity and strong performance in sequence modeling.
            In this work, we propose the Mamba Policy, a <b><i>lighter but stronger</i></b> policy that reduces the parameter count by over <b>80%</b> compared to the original policy network while achieving superior performance. 
            Specifically, we introduce the XMamba Block, which effectively integrates input information with conditional features and leverages a combination of Mamba and Attention mechanisms for deep feature extraction. 
            Extensive experiments demonstrate that the Mamba Policy excels on the Adroit, Dexart, and MetaWorld datasets, requiring significantly fewer computational resources. 
            Additionally, we highlight the Mamba Policy's enhanced robustness in long-horizon scenarios compared to baseline methods and explore the performance of various Mamba variants within the Mamba Policy framework.
          </p>
        </div>
      </div>
    </div>
    <!--/ Abstract. -->

  </div>
</section>


<section class="section">
  <div class="container is-max-desktop">

    <!-- Model -->
    <div class="columns is-centered">
      <div class="column is-full-width">
        <h2 class="title is-3">Model</h2>

        <!-- Scene representation -->
        <div style="width: 90%; margin: 0 auto;">
          <img src="./assets/main_final.png" style="width: 100%">
        </div>
        <br>
        <br>
        <div class="content has-text-justified">
          <p>
            <b>Overview of Mamba Policy.</b> Our proposed model takes the noised action and the condition as inputs, the latter of which is composed of three parts: 
            point cloud perception embedding, robot state embedding, and time embedding. Each of these components is processed through its respective encoder. 
            The X-Mamba UNet is then employed to process these inputs and ultimately return the predicted noise. During training, the model is updated using MSE loss with the label noise. 
            For validation, the model leverages DDIM to reconstruct the original action, which is then used to interact with the environment and execute different tasks.
          </p>
        </div>
        <!--/ Scene representation -->
        <br/>

        <!--/ Model pipeline -->
        <!--/ Unified sequence -->

      </div>
    </div>
    <!--/ Model -->

    <!-- Exp -->
    <div class="columns is-centered">
      <div class="column is-full-width">
        <h2 class="title is-3">Experiment Results</h2>
        <br>
        <h3 class="title is-4">Efficiency Analysis</h3>
        <!-- Scene representation -->
        
        <div style="width: 80%; margin: 0 auto;">
          <img src="./assets/teaser.png" style="width: 75% ; display: block; margin: 0 auto;">
        </div>
        <br>
        <div class="content has-text-justified">
          <p>
            <b>Efficieny Comparisons.</b> Compared with DP3, our proposed Mamba Policy not only achieves superior results but also gains up to 90% and 80% computational savings regarding FLOPs and parameters number .
          </p>
        </div>
        <!--/ Scene representation -->
        <br/>

        <!-- Scene representation -->
        <h3 class="title is-4">Main Results</h3>
        <div style="width: 99%; margin: 0 auto;">
          <img src="./assets/exp_table.png" style="width: 100%">
        </div>
        <br>
        <div class="content has-text-justified">
          <p>
            <b>Quantitative Comparisons of different baselines in simulation environments.</b> We compare our Mamba Policy with IBC, BCRNN, 3D Diffusion Policy, and Diffusion Policy in Adroit, MetaWorld and DexArt datasets. 
            † denotes our reproduced results for fair comparison. Our proposed Mamba Policy achieves superior results across all domains. 
          </p>
        </div>
        <!--/ Scene representation -->

        <!--/ Model pipeline -->
        <!--/ Unified sequence -->
        <h3 class="title is-4">Ablations on SSM Variants</h3>
        <div style="width: 99%; margin: 0 auto;">
          <img src="./assets/ssm.png" style="width: 60%; display: block; margin: 0 auto;">
        </div>
        <br>
        <div class="content has-text-justified">
          <p>
            <b>Mamba Policy with different SSM variants.</b> We include Mamba, Mamba2, bidirectional SSM, and Hydra for comparisons, where the V1-based and Hydra-based policy achieves good performances.
          </p>
        </div>
        
      </div>
    </div>
    <br>
    <br>

    <!-- Demo -->
    <div class="columns is-centered">
      <div class="column is-full-width" id="demo">
        <h2 class="title is-3">Demo</h2>
        <!-- EAI -->
        <div class="content-block">
          <div style="text-align: center; margin-bottom: 0.5em;"><b>Robotic Manipulation</b></div>
          <div class="task">
            <table class="table is-bordered my-table">
              <tbody>
               <tr>
                  <td style="text-align: center; padding: 0;">
                    <video controls="controls" autoplay muted loop style="max-width: 100%; height: auto;">
                      <source src="assets/mamba-hammer-video-128.mp4" type="video/mp4">
                    </video>
                  </td>
                  <td style="text-align: center; padding: 0;">
                    <video controls="controls" autoplay muted loop style="max-width: 100%; height: auto;">
                      <source src="assets/mamba-pen-video-128.mp4" type="video/mp4">
                    </video>
                  </td>
                  <td style="text-align: center; padding: 0;">
                    <video controls="controls" autoplay muted loop style="max-width: 100%; height: auto;">
                      <source src="assets/mamba-door-video-128.mp4" type="video/mp4">
                    </video>
                  </td>
                  <td style="text-align: center; padding: 0;">
                    <video controls="controls" autoplay muted loop style="max-width: 100%; height: auto;">
                      <source src="assets/mamba-assembly-video-128.mp4" type="video/mp4">
                    </video>
                  </td>
                </tr>
                <tr>
                  <td style="text-align: center; padding: 0;">
                    <video controls="controls" autoplay muted loop style="max-width: 100%; height: auto;">
                      <source src="assets/mamba-faucet-video-128.mp4" type="video/mp4">
                    </video>
                  </td>
                  <td style="text-align: center; padding: 0;">
                    <video controls="controls" autoplay muted loop style="max-width: 100%; height: auto;">
                      <source src="assets/mamba-bucket-video-128.mp4" type="video/mp4">
                    </video>
                  </td>
                  <td style="text-align: center; padding: 0;">
                    <video controls="controls" autoplay muted loop style="max-width: 100%; height: auto;">
                      <source src="assets/mamba-laptop-video-128.mp4" type="video/mp4">
                    </video>
                  </td>
                  <td style="text-align: center; padding: 0;">
                    <video controls="controls" autoplay muted loop style="max-width: 100%; height: auto;">
                      <source src="assets/mamba-toilet-video-128.mp4" type="video/mp4">
                    </video>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        <!--/ EAI -->

      </div>
        <div class="content has-text-justified" style="margin-top: 10px">
               We perform our experiments using the Adroit, MetaWorld, and DexArt benchmarks. These tasks span from simple pick or push tasks to more complex challenges like dexterous manipulation, ensuring the model's effectiveness across a wide range of scenarios.
        </div>
    </div>
    <!--/ Demo -->

  </div>
</section>


<section class="section" id="BibTeX">
  <div class="container is-max-desktop content">
    <h2 class="title">BibTeX</h2>
    <pre><code class="language-bibtex">@article{cao2024mamba,
  title={Mamba Policy: Towards Efficient 3D Diffusion Policy with Hybrid Selective State Models},
  author={Cao, Jiahang and Zhang, Qiang and Sun, Jingkai and Wang, Jiaxu and Cheng, Hao and Li, Yulin and Ma, Jun and Shao, Yecheng and Zhao, Wen and Han, Gang and others},
  journal={arXiv preprint arXiv:2409.07163},
  year={2024}
}</code></pre>
  </div>
</section>

<footer class="footer">
  <div class="container">
    <div class="columns is-centered">
      <div class="column is-8">
        <div class="content">
        <p>
            This website is licensed under a <a rel="license"
            href="http://creativecommons.org/licenses/by-sa/4.0/">Creative
            Commons Attribution-ShareAlike 4.0 International License</a>.
        </p>
        <p>
            Template borrowed from <a href="https://github.com/nerfies/nerfies.github.io">Nerfies</a>.
        </p>
        </div>
      </div>
    </div>
  </div>
</footer>

</body>


<!-- visualization code borrowed from SceneDiffuser -->
<script type="module">

  import * as THREE from 'three'
  import { OrbitControls } from 'three/addons/controls/OrbitControls.js'
  import {GLTFLoader} from 'three/addons/loaders/GLTFLoader.js'

  let canvas1 = document.querySelector('#webgl_pose')
  let scene1 = new THREE.Scene()
  let assetLoader1 = new GLTFLoader()
  let model1

  let camera1 = new THREE.PerspectiveCamera(45, 1.618 / 1.0, 0.1, 100)
  camera1.position.set(5.2, 3.9, -3.9)
  let grid1 = new THREE.GridHelper(30, 30)
  scene1.add(camera1)
  scene1.add(grid1)
  for (let i = 0; i <= 1; i++) {
    for (let k = 0; k <= 1; k++) {
      let spotLight = new THREE.SpotLight(0xAAAAAA)
      spotLight.position.set(50 * (i * 2 - 1), 100, 100 * (k * 2 - 1))
      scene1.add(spotLight)
    }
  }

  let controls1 = new OrbitControls(camera1, canvas1)
  controls1.enableZoom = true
  // controls2.enableDamping = true
  controls1.object.position.set(camera1.position.x, camera1.position.y, camera1.position.z)
  controls1.target = new THREE.Vector3(0, 0, 0)
  controls1.update()

  let renderer1 = new THREE.WebGLRenderer({
      canvas: canvas1,
      alpha: true,
  })
  renderer1.setPixelRatio(Math.min(window.devicePixelRatio, 2))
  renderer1.outputEncoding = THREE.sRGBEncoding
  renderer1.setAnimationLoop(() => {
    renderer1.render(scene1, camera1)
  });

  const radioButtons = document.querySelectorAll('input[name="capability"]')
  for (const radioButton of radioButtons) {
    radioButton.addEventListener('change', (e) => {
      var capability = radioButton.value;
      console.log("capability", capability)
      for (const rb of radioButtons) {rb.disabled = true}

      var img1 = document.getElementById("scene1");
      var img2 = document.getElementById("scene2");
      var img3 = document.getElementById("scene3");
      var img4 = document.getElementById("scene4");
      var img5 = document.getElementById("scene5");
      var img6 = document.getElementById("scene6");

      if (capability == "captioning") {
        img1.src = "assets/scene_view/captioning1.jpg"
        img1.alt = "State what you notice at the pointed location in the 3D representation. " +
                   "[sep] A donut with pink icing and sprinkles."

        img2.src = "assets/scene_view/captioning2.jpg"
        img2.alt = "Explain the object at the pinpointed location in the 3D setting. " +
                   "[sep] A wooden chest of drawers."

        img3.src = "assets/scene_view/captioning3.jpg"
        img3.alt = "Develop a description of the object at the specified position in the 3D backdrop. " +
                   "[sep] This is a black keyboard. It is in front of two monitors."

        img4.src = "assets/scene_view/captioning4.jpg"
        img4.alt = "How does the object appear at the marked location in the 3D view? " +
                   "[sep] This is a white pillow. It is on the right side of the bed."
        img4.style.width = "100%";
        img4.onclick = function() {populateDemo(img4)};

        img5.src = "assets/scene_view/captioning5.jpg"
        img5.alt = "Relay a brief summary of this scene's elements. " +
                   "[sep] In this scene, there is a tall wardrobe standing on the wooden floor. " +
                   "The wardrobe is to the right of a bed and in front of a nightstand. " +
                   "It is also behind a chair and to the left of another chair. " +
                   "There are pillows lying on the bed, with different sizes and colors. " +
                   "Some pillows are close to each other, while others are far away from each other. " +
                   "A blanket is lying on the bed, close to one of the pillows. " +
                   "The room has white walls attached to the floor, giving it an architectural feel. " +
                   "The ceiling is overhead and attached to the walls. " +
                   "There is a curtain hanging on one of the walls, adding a touch of elegance to the room. " +
                   "Overall, this room provides a cozy and comfortable space for relaxation and sleep."
        img5.style.width = "100%";
        img5.onclick = function() {populateDemo(img5)};

        img6.src = "assets/scene_view/captioning6.jpg"
        img6.alt = "Can you walk me through the details of this scene? " +
                   "[sep] In this scene, there is a white rectangular metal heater connected to the wall. " +
                   "It is in front of a curtain and to the right of another curtain. " +
                   "There is also a closed glass window attached to the wall. " +
                   "The floor is made of wooden material and has a striped texture. " +
                   "There are several chairs in the room, with different sizes and colors. " +
                   "Some chairs are close to tables, while others are far away from them. " +
                   "The walls are flat and white, giving the room an architectural feel. " +
                   "The room appears to be a living room or dining area, with comfortable seating options and a cozy atmosphere."
        img6.style.width = "100%";
        img6.onclick = function() {populateDemo(img6)};
      }

      else if (capability == "reasoning") {
        img1.src = "assets/scene_view/reasoning1.jpg"
        img1.alt = "In what part of the shower is the seat located? " +
                   "[sep] Right side of shower."

        img2.src = "assets/scene_view/reasoning2.jpg"
        img2.alt = "How many monitors sit on top of the desk? " +
                   "[sep] 2."

        img3.src = "assets/scene_view/reasoning3.jpg"
        img3.alt = "You are standing in between the toilet on your right and the sink on your left. " +
                   "Is the door closed or open to your left side? " +
                   "[sep] Closed."

        img4.src = "assets/scene_view/reasoning4.jpg"
        img4.alt = "You are sitting on a chair drinking from your bottle. " +
                   "What are you sitting at? " +
                   "[sep] Desk."
        img4.style.width = "100%";
        img4.onclick = function() {populateDemo(img4)};

        img5.src = "assets/scene_view/reasoning5.jpg"
        img5.alt = "What is the type of the room? " +
                   "[sep] Laundry room."
        img5.style.width = "100%";
        img5.onclick = function() {populateDemo(img5)};

        img6.src = "assets/scene_view/reasoning6.jpg"
        img6.alt = "Where is the player? " +
                   "[sep] Standing on the commode."
        img6.style.width = "100%";
        img6.onclick = function() {populateDemo(img6)};
      }

      else if (capability == "dialogue") {
        img1.src = "assets/scene_view/dialogue1.jpg"
        img1.alt = "I'm redecorating my bedroom and I need some advice. Can you help me? " +
                   "[sep] Of course! I'd be happy to help. What do you need advice on? " +
                   "[SEP] I want to add some color to the room. Are there any colorful decorations or objects in the room? " +
                   "[sep] Yes, there is a colorful picture hanging on the wall. It is in front of a shelf and behind an armchair. It adds a nice pop of color to the room. " +
                   "[SEP] That sounds great! What other decorations or furniture are in the room? " +
                   "[sep] There is a wardrobe to the right of a stuffed animal. It is also close to a light and in front of a doorframe. Additionally, there are shelve attached to the wall, with various item displayed on them. " +
                   "[SEP] Are there any curtains in the room? " +
                   "[sep] Yes, there are curtain hanging on the wall. They are to the left of some shelve and have a patterned texture. " +
                   "[SEP] Thank you for the information! It's really helpful for my redecorating plans. " +
                   "[sep] You're welcome! I'm glad I could help. If you have any more questions or need further assistance, feel free to ask."

        img2.src = "assets/scene_view/dialogue2.jpg"
        img2.alt = "Can you help me find the blackboard in the room? " +
                   "[sep] Yes, there is a blackboard in the room. It is located on one of the wall. " +
                   "[SEP] Thank you. I also need a chair to sit on. Where can I find a chair? " +
                   "[sep] There are several chair in the room. One chair is close to the desk, another chair is next to the radiator, and there are more chair near the table. " +
                   "[SEP] Great! I need a wide chair. " +
                   "[sep] The chair next to the desk is wide in size. You can sit on that chair. " +
                   "[SEP] Thank you for your help. " +
                   "[sep] You're welcome! If you have any other questions, feel free to ask."

        img3.src = "assets/scene_view/dialogue3.jpg"
        img3.alt = "Can you tell me about the cleanliness of the bathroom? " +
                   "[sep] Sure! The floor of the bathroom is clean and it has a tiled texture. There is a towel hanging on the wall, and it is tidy. " +
                   "[SEP] Is the toilet seat down or up? " +
                   "[sep] The toilet seat is down. " +
                   "[SEP] Is there a mirror in the bathroom? " +
                   "[sep] Yes, there is a mirror in the bathroom. " +
                   "[SEP] Is there any shampoo in the bathroom? " +
                   "[sep] Yes, there is a shampoo in the room. " +
                   "[SEP] Thank you for the information. " +
                   "[sep] You're welcome! If you have any more questions, feel free to ask."

        img4.style.width = 0
        img4.onclick = null

        img5.style.width = 0
        img5.onclick = null

        img6.style.width = 0
        img6.onclick = null
      }

      else if (capability == "planning") {
        img1.src = "assets/scene_view/planning1.jpg"
        img1.alt = "Help in setting up the task's plan: Tidy up the bedroom and neaten the clutter. " +
                   "[sep] 1. Make the bed by straightening the sheets, fluffing the pillows, and arranging the blankets.\n" +
                   "2. Vacuum or sweep the floor to remove any dirt or debris.\n" +
                   "3. Dust and clean the surfaces of the furniture, such as the desk, shelves, and wardrobe.\n" +
                   "4. Organize the items on the shelves, ensuring that they are neatly arranged.\n" +
                   "5. Remove any clutter or unnecessary items from the room.\n" +
                   "6. Check and adjust the temperature or settings of the heater if necessary."

        img2.src = "assets/scene_view/planning2.jpg"
        img2.alt = "State a plan for the task: Arrange the study room. " +
                   "[sep] 1. Dust and clean the surfaces of the furniture, such as the desk, shelves, and wardrobe.\n" +
                   "2. Organize the items on the desk, ensuring that they are neatly arranged and easily accessible.\n" +
                   "3. Adjust the curtains to the desired position, allowing natural light to enter the room.\n" +
                   "4. Arrange any personal belongings or decor items on the shelves, creating a visually pleasing display.\n" +
                   "5. Check and adjust the temperature or settings of the heater if necessary."

        img3.src = "assets/scene_view/planning3.jpg"
        img3.alt = "Can you depict a plan for the task: Arrange the furniture in the living room. " +
                   "[sep] 1. Clean the floor by sweeping or vacuuming to remove any dirt or debris.\n" +
                   "2. Arrange the chairs around the table, ensuring they are evenly spaced and facing the table.\n" +
                   "3. Adjust the position of the window blinds or curtains to control the amount of light entering the room.\n" +
                   "4. Organize the items on the shelves, ensuring they are neatly arranged and visually appealing.\n" +
                   "5. Turn on the lights in the living room to create a bright and inviting atmosphere."

        img4.style.width = 0
        img4.onclick = null

        img5.style.width = 0
        img5.onclick = null

        img6.style.width = 0
        img6.onclick = null
      }

      for (const rb of radioButtons) {rb.disabled = false}
    })
  }
  radioButtons[0].click()

  // resize renderers
  function resizeRenderers() {
    let content_width = document.querySelector('#demo').offsetWidth
    renderer1.setSize(content_width * 6.7 / 12, content_width / 1.618 * 6.7 / 12)
  }
  window.addEventListener('resize', () => {
    resizeRenderers()
  })
  resizeRenderers()

  window.model1 = model1;
  window.scene1 = scene1;
  window.assetLoader1 = assetLoader1;

</script>


</html>
