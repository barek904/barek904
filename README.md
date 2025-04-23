- 👋 Hi, I’m @barek904
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
barek904/barek904 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8">
  <title>Gra o Sianokosach 3D</title>
  <style>
    body { margin: 0; overflow: hidden; }
    canvas { display: block; }
  </style>
</head>
<body>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
  <script>
    let scene, camera, renderer, tractor, ground, hayBales = 0;
    let tractorSpeed = 0.1;
    let tractorRotationSpeed = 0.05;
    let terrainCost = 1000; // koszt kupna nowego terenu
    let money = 1000; // początkowa ilość pieniędzy
    let terrainPurchased = false; // stan kupienia terenu

    function init() {
      // Tworzymy scenę
      scene = new THREE.Scene();
      scene.background = new THREE.Color(0x87ceeb); // niebo

      // Ustawienie kamery
      camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      camera.position.z = 5;
      camera.position.y = 2;
      camera.rotation.x = -0.3;

      // Tworzymy renderera
      renderer = new THREE.WebGLRenderer();
      renderer.setSize(window.innerWidth, window.innerHeight);
      document.body.appendChild(renderer.domElement);

      // Tworzymy ziemię (płaskie pole)
      const geometry = new THREE.PlaneGeometry(100, 100);
      const material = new THREE.MeshBasicMaterial({ color: 0x00b300, side: THREE.DoubleSide });
      ground = new THREE.Mesh(geometry, material);
      ground.rotation.x = Math.PI / 2;
      scene.add(ground);

      // Tworzymy traktor
      const tractorGeometry = new THREE.BoxGeometry(1, 0.5, 0.5);
      const tractorMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 });
      tractor = new THREE.Mesh(tractorGeometry, tractorMaterial);
      tractor.position.y = 0.25;
      scene.add(tractor);

      // Dodajemy oświetlenie
      const light = new THREE.AmbientLight(0x404040); // światło otoczenia
      scene.add(light);

      // Ruch traktora
      window.addEventListener('keydown', onKeyDown);

      // Uruchomienie animacji
      animate();
    }

    function onKeyDown(event) {
      if (event.key === 'w') {
        tractor.position.z -= tractorSpeed;
      } else if (event.key === 's') {
        tractor.position.z += tractorSpeed;
      } else if (event.key === 'a') {
        tractor.position.x -= tractorSpeed;
      } else if (event.key === 'd') {
        tractor.position.x += tractorSpeed;
      } else if (event.key === 'r') {
        tractor.rotation.y -= tractorRotationSpeed;
      } else if (event.key === 'f') {
        tractor.rotation.y += tractorRotationSpeed;
      } else if (event.key === '1') {
        // Kupowanie terenu (koszt 1000 monet)
        if (money >= terrainCost && !terrainPurchased) {
          money -= terrainCost;
          terrainPurchased = true;
          alert("Teren został kupiony!");
        } else {
          alert("Nie masz wystarczającej ilości monet!");
        }
      } else if (event.key === '2') {
        // Robienie belek
        if (terrainPurchased) {
          hayBales++;
          alert("Stworzono belkę! Masz teraz " + hayBales + " belek.");
        } else {
          alert("Musisz najpierw kupić teren!");
        }
      }
    }

    function animate() {
      requestAnimationFrame(animate);

      // Rysowanie sceny
      renderer.render(scene, camera);
    }

    init();
  </script>
</body>
</html>
