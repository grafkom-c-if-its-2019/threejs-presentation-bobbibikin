# Tutorial Material Grafkom

Tutorial ini akan membahas tentang basic dari Material. Nanti akan dicontohkan tentang bagaimana fungsi-fungsi basic berjalan dan bagaimana pengaruhnya.

## Deklarasi library
Pertama-tama kita perlu memanggil library-library yang akan di pakai.

```javascript
<script type="text/javascript" src="/Tutorial/three.js"></script>
    <script type="text/javascript" src="/Tutorial/stats.js"></script>
    <script type="text/javascript" src="/Tutorial/CanvasRenderer.js"></script>
    <script type="text/javascript" src="/Tutorial/Projector.js"></script>
```
File dari masing-masing library bisa di download pada folder github ini

### Body
```javascript
    <div id="Stats-output"> </div>
    <!-- Div which will hold the Output -->
    <div id="WebGL-output"></div>
    <script>
        function init(){
            var stats = initStats();
            
            // create a scene, that will hold all our elements such as objects, cameras and lights.
            var scene = new THREE.Scene();

            // create a camera, which defines where we're looking at.
            var camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);

            // create a render and set the size
            var renderer;
            var webGLRenderer = new THREE.WebGLRenderer();
            webGLRenderer.setClearColor(new THREE.Color(0xEEEEEE, 1.0));
            webGLRenderer.setSize(window.innerWidth, window.innerHeight);
            webGLRenderer.shadowMapEnabled = true;

            var canvasRenderer = new THREE.CanvasRenderer();
            canvasRenderer.setSize(window.innerWidth, window.innerHeight);
            renderer = webGLRenderer;

            var sphereGeometry = new THREE.SphereGeometry(14, 20, 20);
            var cubeGeometry = new THREE.BoxGeometry(15, 15, 15);
            var planeGeometry = new THREE.PlaneGeometry(14, 14, 4, 4);

            //ubah material disini ya
            var meshMaterial = new THREE.MeshBasicMaterial({color: 0x7777ff});

            var sphere = new THREE.Mesh(sphereGeometry, meshMaterial);
            var cube = new THREE.Mesh(cubeGeometry, meshMaterial);
            var plane = new THREE.Mesh(planeGeometry, meshMaterial);

            // position the sphere
            sphere.position.x = 0;
            sphere.position.y = 0;
            sphere.position.z = 0;


            cube.position = sphere.position;
            plane.position = sphere.position;


            // add the sphere to the scene
            scene.add(cube);

            // position and point the camera to the center of the scene
            camera.position.x = -20;
            camera.position.y = 50;
            camera.position.z = 40;
            camera.lookAt(new THREE.Vector3(10, 0, 0));

            // add subtle ambient lighting
            var ambientLight = new THREE.AmbientLight(0x0c0c0c);
            scene.add(ambientLight);

            // add spotlight for the shadows
            var spotLight = new THREE.SpotLight(0xffffff);
            spotLight.position.set(-40, 60, -10);
            spotLight.castShadow = true;
            scene.add(spotLight);

            // add the output of the renderer to the html element
            document.getElementById("WebGL-output").appendChild(renderer.domElement);

            // call the render function
            var step = 0;
            var oldContext = null;
            
            //edit-edit disini ya
 
            render();

            function render() {
                stats.update();

                cube.rotation.y = step += 0.01;
                plane.rotation.y = step;
                sphere.rotation.y = step;

                // render using requestAnimationFrame
                requestAnimationFrame(render);
                renderer.render(scene, camera);
            }

            function initStats() {

                var stats = new Stats();

                stats.setMode(0); // 0: fps, 1: ms


                // Align top-left
                stats.domElement.style.position = 'absolute';
                stats.domElement.style.left = '0px';
                stats.domElement.style.top = '0px';

                document.getElementById("Stats-output").appendChild(stats.domElement);

                return stats;
            }
        }
        window.onload = init;
        </script>
```
Di dalam body, akan ada canvas untuk tempat membuat kubus/lingkaran/shape nanti. Kita perlu mendifiniskan posisi dan jenis material apa yang akan digunakan dalam pembuatan bentuk. Kita juga perlu mendefiniskan posisi camera, dan juga spotlight serta ambientlight yang sudah dijelaskan di bab sebelumnya.

### MeshBasicMaterial
Untuk membuat material dengan tipe basic. Kita perlu mengubah kodingan yang di atas di bagian ini
```javascript
 var meshMaterial = new THREE.MeshBasicMaterial({color: 0x7777ff});
```
Kemudian untuk mengambar kubus, kita perlu menambahkan
```javascript
scene.add(cube);
```
Hasilnya akan menjadi seperti ini:
![Gambar1](/Tutorial/images/foto1.png)
Kita juga bisa mengubah-ubah bentuk yang mau dibuat. Kita bisa membuat lingkaran dengan cara mengubah cube menjadi *sphere*. Kita juga bisa membuat plane dengan cara mengubah cube menjadi *plane*.


Setelah itu kita mulai masuk ke fungsi-fungsi dasar material:
```javascript
meshMaterial.transparent=true;
meshMaterial.opacity = 0.2;
meshMaterial.wireframe = 1;
meshMaterial.visible = true;
meshMaterial.side = THREE.FrontSide;
meshMaterial.side = THREE.BackSide;
meshMaterial.side = THREE.DoubleSide;
```
Transparan sesuai dengan nama fungsinya membuat gambar menjadi transparan. Dalam transparan kita bisa mengubah *opacity*nya sehingga gambar menjadi sedikit tidak terlihat.

Wireframe digunakan untuk mengaktifkan wireframenya. Dan visible untuk mengubah gambar menjadi terlihat atau tidak.


### Jenis-jenis material
Pada bagian diatas kita sudah mencoba membuat sebuah kubus dengan material basic. Sekarang kita coba untuk membuat kubus dengan material lambert dan phong. Caranya adalah kita perlu mengubah kodingannya menjadi seperti ini
```javascript
var meshMaterial = new THREE.MeshLambertMaterial({color: 0x7777ff});
//atau
var meshMaterial = new THREE.MeshPhongMaterial({color: 0x7777ff});
```
Berikut ini adalah gambar lingkaran dengan material lambert
![Gambar2](/Tutorial/images/foto2.png)
Berikut ini adalah gambar lingkaran dengan material phong
![Gambar3](/Tutorial/images/foto3.png)
Bisa dilihat bahwa perbedaan lambert dan phong adalah di dalam phong ada cahaya yang keluar dari permukaan

### Fungsi khusus Phong dan Lambert
Phong dan Lambert memiliki beberapa fungsi khusus untuk mengaturnya. Contohnya adalah Shininess dan metal untuk phong.
Kita perlu menambahkan kodingan ini:
```Javascript
meshMaterial.shininess = 100;
meshMaterial.metal = true;
```
*shininess* berguna untuk menggambarkan seberapa mengkilau (shiny) objek tersebut.

*metal* berguna untuk sedikit mengubah metode perhitungan warna dari pixelnya agar terlihat sedikit lebih *metal*. (dengan catatan perubahan yang terlihat sangant minimum).    