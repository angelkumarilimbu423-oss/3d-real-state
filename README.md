 <html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Architect 3D | Procedural Generator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
</head>
<body class="bg-slate-900 text-white font-sans overflow-x-hidden">

    <nav class="p-6 border-b border-slate-800 flex justify-between items-center bg-slate-900/50 backdrop-blur-md sticky top-0 z-50">
        <h1 class="text-xl font-bold tracking-tighter text-blue-400 uppercase italic">AI.Architect v1.0</h1>
        <div class="flex items-center gap-4">
            <span class="text-[10px] font-mono text-slate-500 uppercase tracking-widest">Procedural Mesh Engine</span>
            <button class="bg-blue-600 hover:bg-blue-500 text-white px-4 py-2 rounded-md text-sm font-bold transition">Export .OBJ</button>
        </div>
    </nav>

    <div class="grid grid-cols-1 lg:grid-cols-4 h-[calc(100vh-80px)]">
        
        <div class="p-6 border-r border-slate-800 bg-slate-900 overflow-y-auto">
            <h2 class="text-sm font-bold text-slate-400 mb-6 uppercase tracking-widest italic">Building Parameters</h2>
            
            <div class="space-y-6">
                <div>
                    <label class="block text-xs mb-2 text-slate-500 uppercase">Building Height (Floors)</label>
                    <input type="range" id="floors" min="1" max="50" value="12" class="w-full h-1 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-blue-500">
                    <div class="flex justify-between text-[10px] mt-2 font-mono text-blue-400"><span id="floor-val">12</span><span>Floors</span></div>
                </div>

                <div>
                    <label class="block text-xs mb-2 text-slate-500 uppercase">Footprint (Width/Depth)</label>
                    <input type="range" id="size" min="5" max="30" value="15" class="w-full h-1 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-blue-500">
                    <div class="flex justify-between text-[10px] mt-2 font-mono text-blue-400"><span id="size-val">15</span><span>Meters</span></div>
                </div>

                <div>
                    <label class="block text-xs mb-2 text-slate-500 uppercase">Facade Transparency</label>
                    <input type="range" id="glass" min="0" max="100" value="60" class="w-full h-1 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-blue-500">
                </div>

                <div class="pt-6 border-t border-slate-800">
                    <button onclick="rebuildBuilding()" class="w-full bg-blue-600/10 border border-blue-500/50 text-blue-400 py-3 rounded-lg font-bold hover:bg-blue-600 hover:text-white transition uppercase text-xs tracking-widest">
                        Apply AI Logic
                    </button>
                </div>
            </div>

            <div class="mt-10 p-4 bg-blue-500/5 rounded-xl border border-blue-500/20">
                <p class="text-[10px] text-blue-400 font-mono mb-2 uppercase">AI Analysis</p>
                <p id="ai-status" class="text-xs text-slate-400 leading-relaxed italic">"Optimizing structural load for a 12-story commercial unit..."</p>
            </div>
        </div>

        <div class="lg:col-span-3 relative bg-black">
            <div id="canvas-container" class="w-full h-full cursor-move"></div>
            
            <div class="absolute bottom-6 left-6 pointer-events-none">
                <div class="bg-black/40 backdrop-blur-md p-4 rounded border border-white/10">
                    <p class="text-[10px] text-slate-500 font-mono uppercase">Current Mesh Statistics</p>
                    <p id="mesh-stats" class="text-sm font-bold text-white uppercase tracking-tighter">Vertices: 450 | Draw Calls: 1</p>
                </div>
            </div>
        </div>
    </div>

    <script>
        let scene, camera, renderer, controls, buildingGroup;

        function init() {
            scene = new THREE.Scene();
            scene.background = new THREE.Color(0x0a0a0f);
            scene.fog = new THREE.Fog(0x0a0a0f, 10, 100);

            camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.set(40, 40, 40);

            renderer = new THREE.WebGLRenderer({ antialias: true });
            renderer.setSize(window.innerWidth * 0.75, window.innerHeight - 80);
            document.getElementById('canvas-container').appendChild(renderer.domElement);

            controls = new THREE.OrbitControls(camera, renderer.domElement);
            controls.enableDamping = true;

            const light = new THREE.DirectionalLight(0xffffff, 1);
            light.position.set(10, 20, 10);
            scene.add(light);
            scene.add(new THREE.AmbientLight(0x404040, 2));

            // Grid Helper
            const grid = new THREE.GridHelper(100, 20, 0x303030, 0x101010);
            scene.add(grid);

            buildingGroup = new THREE.Group();
            scene.add(buildingGroup);

            rebuildBuilding();
            animate();
        }

        function rebuildBuilding() {
            // Clear existing building
            while(buildingGroup.children.length > 0){ 
                buildingGroup.remove(buildingGroup.children[0]); 
            }

            const floors = parseInt(document.getElementById('floors').value);
            const size = parseInt(document.getElementById('size').value);
            const glass = parseInt(document.getElementById('glass').value) / 100;
            
            // Update UI
            document.getElementById('floor-val').innerText = floors;
            document.getElementById('size-val').innerText = size;
            document.getElementById('ai-status').innerText = `"Building geometry modified. Structural core updated for ${floors} floors with ${size}m footprint."`;

            const floorHeight = 3.5;
            
            for (let i = 0; i < floors; i++) {
                // Building Floor Mesh
                const geometry = new THREE.BoxGeometry(size, floorHeight - 0.2, size);
                const material = new THREE.MeshPhongMaterial({ 
                    color: 0x2563eb, 
                    transparent: true, 
                    opacity: glass,
                    shininess: 100
                });
                const floor = new THREE.Mesh(geometry, material);
                floor.position.y = (i * floorHeight) + (floorHeight / 2);
                
                // Wireframe Edge Effect
                const edges = new THREE.EdgesGeometry(geometry);
                const line = new THREE.LineSegments(edges, new THREE.LineBasicMaterial({ color: 0x60a5fa }));
                line.position.y = floor.position.y;

                buildingGroup.add(floor);
                buildingGroup.add(line);
            }
            
            document.getElementById('mesh-stats').innerText = `Vertices: ${floors * 8} | Floors: ${floors}`;
        }

        function animate() {
            requestAnimationFrame(animate);
            controls.update();
            renderer.render(scene, camera);
        }

        window.addEventListener('resize', () => {
            camera.aspect = (window.innerWidth * 0.75) / (window.innerHeight - 80);
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth * 0.75, window.innerHeight - 80);
        });

        init();
    </script>
</body>
</html>
