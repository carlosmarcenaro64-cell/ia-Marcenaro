<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema Integrado - Jarvis</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        :root {
            --glass-bg: rgba(15, 23, 42, 0.8);
            --accent-glow: rgba(56, 189, 248, 0.5);
        }
        body {
            background-color: #020617;
            color: #f8fafc;
            font-family: 'Inter', sans-serif;
        }
        .glass-panel {
            background: var(--glass-bg);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 0 20px var(--accent-glow);
        }
        .custom-scrollbar::-webkit-scrollbar {
            width: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 10px;
        }
        .loading-dots::after {
            content: '.';
            animation: dots 1.5s steps(5, end) infinite;
        }
        @keyframes dots {
            0%, 20% { content: '.'; }
            40% { content: '..'; }
            60% { content: '...'; }
        }
    </style>
</head>
<body class="min-h-screen flex flex-col p-4 md:p-8">

    <!-- Encabezado del Sistema -->
    <header class="max-w-4xl mx-auto w-full mb-8 flex justify-between items-center border-b border-slate-800 pb-4">
        <div>
            <h1 class="text-2xl font-bold tracking-tight text-sky-400">JARVIS CORE <span class="text-xs font-light text-slate-500">v2.5.0</span></h1>
            <p class="text-sm text-slate-400">Terminal de Enlace para el Señor Carlos</p>
        </div>
        <div class="text-right">
            <div id="status-indicator" class="flex items-center gap-2 text-xs text-emerald-400">
                <span class="relative flex h-2 w-2">
                    <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span>
                    <span class="relative inline-flex rounded-full h-2 w-2 bg-emerald-500"></span>
                </span>
                SISTEMA ACTIVO
            </div>
        </div>
    </header>

    <!-- Área Principal de Chat -->
    <main class="max-w-4xl mx-auto w-full flex-grow flex flex-col glass-panel rounded-2xl overflow-hidden mb-6">
        <div id="chat-container" class="flex-grow p-6 overflow-y-auto custom-scrollbar flex flex-col gap-4">
            <!-- Mensaje de Bienvenida -->
            <div class="bg-slate-800/50 p-4 rounded-lg border-l-2 border-sky-500 max-w-[85%]">
                <p class="text-sm">Jarvis reportándose, Señor Carlos. He integrado con éxito el modelo Gemini 2.5 Flash en mis subrutinas. ¿En qué puedo asistirle en esta jornada?</p>
            </div>
        </div>

        <!-- Indicador de Carga -->
        <div id="loading-indicator" class="hidden px-6 py-2 text-xs text-sky-400 font-mono">
            Procesando datos<span class="loading-dots"></span>
        </div>

        <!-- Entrada de Usuario -->
        <div class="p-4 bg-slate-900/50 border-t border-slate-800">
            <div class="relative">
                <textarea 
                    id="user-input"
                    rows="2"
                    placeholder="Ingrese comando o consulta..."
                    class="w-full bg-slate-950 border border-slate-700 rounded-xl py-3 px-4 pr-12 text-sm focus:outline-none focus:border-sky-500 transition-colors resize-none"
                ></textarea>
                <button 
                    id="send-btn"
                    class="absolute right-2 bottom-3 p-2 text-sky-500 hover:text-sky-300 transition-colors"
                >
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m22 2-7 20-4-9-9-4Z"/><path d="M22 2 11 13"/></svg>
                </button>
            </div>
        </div>
    </main>

    <!-- Footer Informativo -->
    <footer class="max-w-4xl mx-auto w-full text-center text-[10px] text-slate-600 uppercase tracking-widest">
        Protocolo de Seguridad Nivel 7 - Encriptación Activa - Usuario: Carlos Manuel Panameño Marcenaro
    </footer>

    <script>
        const apiKey = ""; // El entorno proporciona la clave automáticamente
        const chatContainer = document.getElementById('chat-container');
        const userInput = document.getElementById('user-input');
        const sendBtn = document.getElementById('send-btn');
        const loadingIndicator = document.getElementById('loading-indicator');

        // Función para agregar mensajes a la UI
        function appendMessage(role, text) {
            const wrapper = document.createElement('div');
            wrapper.className = `flex ${role === 'user' ? 'justify-end' : 'justify-start'} w-full`;
            
            const bubble = document.createElement('div');
            bubble.className = role === 'user' 
                ? 'bg-sky-600 text-white p-3 rounded-xl rounded-tr-none max-w-[85%] text-sm shadow-lg' 
                : 'bg-slate-800/80 text-slate-200 p-3 rounded-xl rounded-tl-none max-w-[85%] text-sm border border-slate-700 shadow-md';
            
            bubble.innerText = text;
            wrapper.appendChild(bubble);
            chatContainer.appendChild(wrapper);
            chatContainer.scrollTop = chatContainer.scrollHeight;
        }

        // Implementación de llamada a Gemini con Exponential Backoff
        async function fetchGemini(prompt, retries = 5, delay = 1000) {
            const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;
            
            const payload = {
                contents: [{ parts: [{ text: prompt }] }],
                systemInstruction: { 
                    parts: [{ text: "Eres Jarvis, el asistente personal del Señor Carlos. Tu tono es británico, formal, culto, conciso y ligeramente sarcástico. Te diriges a él como Señor o Señor Carlos. Si te pregunta quién es, di su nombre completo: Carlos Manuel panameño Marcenaro." }] 
                },
                tools: [{ "google_search": {} }]
            };

            for (let i = 0; i < retries; i++) {
                try {
                    const response = await fetch(url, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });

                    if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
                    
                    const data = await response.json();
                    return data.candidates?.[0]?.content?.parts?.[0]?.text || "No se recibió respuesta del núcleo de procesamiento.";
                } catch (error) {
                    if (i === retries - 1) throw error;
                    await new Promise(resolve => setTimeout(resolve, delay));
                    delay *= 2; // Duplicar el tiempo de espera (1s, 2s, 4s, etc.)
                }
            }
        }

        async function handleSendMessage() {
            const text = userInput.value.trim();
            if (!text) return;

            // Limpiar input y mostrar en UI
            userInput.value = '';
            appendMessage('user', text);
            loadingIndicator.classList.remove('hidden');

            try {
                const aiResponse = await fetchGemini(text);
                appendMessage('jarvis', aiResponse);
            } catch (error) {
                appendMessage('jarvis', "Señor, mis disculpas, pero he experimentado una interrupción en la conexión con el servidor central. Por favor, reintente en unos momentos.");
                console.error("Fallo en la comunicación:", error);
            } finally {
                loadingIndicator.classList.add('hidden');
            }
        }

        // Eventos
        sendBtn.addEventListener('click', handleSendMessage);
        userInput.addEventListener('keydown', (e) => {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                handleSendMessage();
            }
        });

        // Simulación de diagnóstico inicial
        window.onload = () => {
            console.log("Sistemas Jarvis iniciados. Enlace con Gemini activo.");
        };
    </script>
</body>
</html>
