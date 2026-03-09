# 3d-real-state
A high-performance, responsive landing page template designed for modern real estate marketing. This project integrates interactive 3D architectural visualization directly into the browser to enhance property showcases.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI LuxuryEstate 3D | The Horizon Tower</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="module" src="https://ajax.googleapis.com/ajax/libs/model-viewer/3.4.0/model-viewer.min.js"></script>
</head>
<body class="bg-gray-50 text-gray-900 font-sans">

    <nav class="p-6 bg-white shadow-sm flex justify-between items-center sticky top-0 z-50">
        <h1 class="text-2xl font-bold text-blue-600 flex items-center gap-2">
            <span class="bg-blue-600 text-white p-1 rounded-lg text-xs">AI</span> LuxuryEstate 3D
        </h1>
        <div class="space-x-4 flex items-center">
            <span class="hidden sm:inline text-xs text-green-500 font-mono animate-pulse">● AI System Active</span>
            <button class="bg-blue-600 text-white px-5 py-2 rounded-lg hover:bg-blue-700 transition font-medium">Contact Agent</button>
        </div>
    </nav>

    <div class="max-w-6xl mx-auto p-6 grid grid-cols-1 md:grid-cols-2 gap-10 mt-10">
        
        <div class="relative group bg-white rounded-2xl shadow-xl overflow-hidden border border-gray-100 h-[500px]">
            <div class="absolute top-4 left-4 z-10 bg-black/60 backdrop-blur-md text-white px-3 py-1 rounded-full text-xs font-mono">
                AI Analysis: Structural Integrity 98%
            </div>
            
            <model-viewer 
                src="https://modelviewer.dev/shared-assets/models/Astronaut.glb" 
                camera-controls 
                auto-rotate 
                ar
                shadow-intensity="1"
                style="width: 100%; height: 100%; background-color: #f9fafb;">
                <button slot="ar-button" class="absolute bottom-4 right-4 bg-white p-2 rounded-lg shadow-md border text-xs font-bold">
                    👋 View in AR
                </button>
            </model-viewer>
        </div>

        <div class="flex flex-col justify-center">
            <div class="inline-block w-fit bg-blue-50 text-blue-600 px-3 py-1 rounded-full text-sm font-bold mb-4">
                ✨ AI-Generated Listing
            </div>
            <h2 class="text-4xl font-bold mb-2">The Horizon Tower</h2>
            <p class="text-xl text-blue-600 font-semibold mb-6">$1,250,000 — Downtown District</p>
            
            <div class="grid grid-cols-2 gap-4 mb-8">
                <div class="bg-blue-50/50 p-4 rounded-xl border border-blue-100">
                    <p class="text-gray-500 text-xs uppercase tracking-wider">AI Yield Est.</p>
                    <p class="text-xl font-bold text-blue-700">+8.4% / yr</p>
                </div>
                <div class="bg-gray-50 p-4 rounded-xl border border-gray-100">
                    <p class="text-gray-500 text-xs uppercase tracking-wider">Smart Rating</p>
                    <p class="text-xl font-bold text-gray-800">Gold Tier</p>
                </div>
            </div>

            <h3 class="text-lg font-bold mb-2">Architectural Insights</h3>
            <p id="ai-description" class="text-gray-600 leading-relaxed mb-6 italic">
                "Our AI has analyzed this 3D model. The glass facade optimizes thermal efficiency by 22%, while the structural core is designed for maximum wind resistance in urban corridors..."
            </p>

            <button onclick="simulateAITour()" class="w-full bg-black text-white py-4 rounded-xl font-bold hover:bg-gray-800 transition flex items-center justify-center gap-2">
                🪄 Start AI Virtual Tour
            </button>
        </div>
    </div>

    <div id="ai-chat" class="fixed bottom-6 right-6 z-50">
        <div id="chat-window" class="hidden w-80 bg-white shadow-2xl rounded-2xl border border-gray-200 mb-4 overflow-hidden">
            <div class="bg-blue-600 p-4 text-white font-bold flex justify-between items-center">
                <span class="flex items-center gap-2">
                    <span class="w-2 h-2 bg-green-400 rounded-full"></span> AI Property Guide
                </span>
                <button onclick="toggleChat()" class="hover:text-gray-200 text-2xl">&times;</button>
            </div>
            <div id="chat-history" class="h-64 p-4 overflow-y-auto text-sm space-y-3 bg-gray-50">
                <div class="bg-white p-3 rounded-lg shadow-sm border border-gray-100">
                    Hello! I've scanned the 3D model for <strong>The Horizon Tower</strong>. Ask me anything about the materials or layout!
                </div>
            </div>
            <div class="p-3 border-t bg-white">
                <input type="text" id="user-input" onkeypress="handleChat(event)" placeholder="Ask AI..." class="w-full border-none outline-none focus:ring-0 text-sm">
            </div>
        </div>
        <button onclick="toggleChat()" class="bg-blue-600 text-white p-4 rounded-full shadow-lg hover:scale-110 transition active:scale-95">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 10h.01M12 10h.01M16 10h.01M9 16H5a2 2 0 01-2-2V6a2 2 0 012-2h14a2 2 0 012 2v8a2 2 0 01-2 2h-5l-5 5v-5z"/></svg>
        </button>
    </div>

    <script>
        function toggleChat() {
            document.getElementById('chat-window').classList.toggle('hidden');
        }

        function handleChat(e) {
            if (e.key === 'Enter' && e.target.value.trim() !== "") {
                const history = document.getElementById('chat-history');
                const userMsg = e.target.value;
                
                // Add User Message
                history.innerHTML += `<div class="bg-blue-100 p-3 rounded-lg text-right text-blue-800 ml-8">${userMsg}</div>`;
                e.target.value = "";

                // Fake AI response
                setTimeout(() => {
                    history.innerHTML += `<div class="bg-white p-3 rounded-lg shadow-sm border border-gray-100 mr-8 italic">AI is thinking...</div>`;
                    history.scrollTop = history.scrollHeight;
                }, 500);
            }
        }

        function simulateAITour() {
            const desc = document.getElementById('ai-description');
            desc.innerText = "AI is currently simulating lighting conditions for 6:00 PM (Sunset)... The panoramic balconies show optimal golden-hour exposure.";
            const btn = event.currentTarget;
            btn.innerHTML = "✨ Simulation Active...";
            setTimeout(() => {
                alert("AI Virtual Tour Started: Scanning 3D Geometry and Environmental Data.");
                btn.innerHTML = "🪄 Start AI Virtual Tour";
            }, 1500);
        }
    </script>
</body>
</html>
// Add this to your <script> section to generate a building procedurally
function generateBuilding(floors, width, depth) {
    const scene = new THREE.Scene();
    const material = new THREE.MeshPhongMaterial({ 
        color: 0x88ccff, 
        transparent: true, 
        opacity: 0.8,
        shininess: 100 
    });

    const floorHeight = 3;
    
    for (let i = 0; i < floors; i++) {
        // Create one floor
        const geometry = new THREE.BoxGeometry(width, floorHeight, depth);
        const floorMesh = new THREE.Mesh(geometry, material);
        
        // Stack floors on top of each other
        floorMesh.position.y = i * floorHeight;
        
        // Add floor "skeleton" or wireframe for AI effect
        const wireframe = new THREE.LineSegments(
            new THREE.EdgesGeometry(geometry),
            new THREE.LineBasicMaterial({ color: 0x0000ff })
        );
        wireframe.position.y = i * floorHeight;
        
        scene.add(floorMesh);
        scene.add(wireframe);
    }
    return scene;
}

// Example: AI creates a 10-floor tower based on user input
const aiBuilding = generateBuilding(10, 15, 15);

