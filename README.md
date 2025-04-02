<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GhostCat Copilot</title>
    <style>
        body {
            background-color: #121212;
            color: white;
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            height: 100vh;
            margin: 0;
        }
        .header {
            padding: 10px;
            text-align: center;
            font-size: 18px;
            color: #00FFFF;
            border-bottom: 1px solid #333;
            position: relative;
        }
        .chat-container {
            flex: 1;
            overflow-y: auto;
            padding: 10px;
        }
        .message {
            background: #2A2A2A;
            padding: 10px;
            border-radius: 10px;
            margin-bottom: 10px;
            width: fit-content;
        }
        .input-container {
            display: flex;
            padding: 10px;
            border-top: 1px solid #333;
        }
        input {
            flex: 1;
            padding: 10px;
            border-radius: 5px;
            border: none;
            background: #333;
            color: white;
            outline: none;
        }
        button {
            margin-left: 10px;
            padding: 10px;
            border-radius: 5px;
            border: none;
            background: #007AFF;
            color: white;
            cursor: pointer;
        }
        .three-dots {
            position: absolute;
            top: 10px;
            right: 10px;
            cursor: pointer;
            font-size: 20px;
            color: #00FFFF;
        }
        .update-option {
            position: absolute;
            top: 40px;
            right: 10px;
            background-color: #333;
            padding: 5px;
            border-radius: 5px;
            color: white;
            font-size: 14px;
            display: none;
        }
        .recommendations {
            display: flex;
            overflow-x: auto;
            padding: 10px;
            margin-bottom: 10px;
            border-bottom: 1px solid #333;
        }
        .recommendation {
            background-color: #333;
            color: white;
            padding: 10px;
            margin-right: 10px;
            border-radius: 5px;
            white-space: nowrap;
        }
        .recommendation a {
            color: #00FFFF;
            text-decoration: none;
        }
        .recommendation a:hover {
            text-decoration: underline;
        }
        .quick-replies {
            display: flex;
            margin-top: 10px;
            gap: 10px;
        }
        .quick-reply {
            background-color: #333;
            color: white;
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
        }
        .quick-reply:hover {
            background-color: #007AFF;
        }
        .repo-link {
            color: #00FFFF;
            cursor: pointer;
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <div class="header">
        GhostCat Copilot
        <span class="three-dots" onclick="toggleUpdateOption()">...</span>
        <div class="update-option" id="updateOption" onclick="clearChat()">Actualizar</div>
    </div>

    <div class="recommendations">
        <div class="recommendation"><a href="https://www.google.com/search?q=mejores+prácticas+de+ciberseguridad" target="_blank">Mejores prácticas de ciberseguridad</a></div>
        <div class="recommendation"><a href="https://www.google.com/search?q=cybersecurity+penetration+testing+tools" target="_blank">Herramientas de pentesting en ciberseguridad</a></div>
        <div class="recommendation"><a href="https://www.google.com/search?q=how+to+prevent+phishing+attacks" target="_blank">Cómo prevenir ataques de phishing</a></div>
    </div>

    <div class="chat-container" id="chat">
        <div class="message" style="background: #007AFF;">
            Hola, soy GhostCat Copilot. ¿En qué puedo ayudarte hoy? Puedes escribir un comando como "comandos" para ver las opciones disponibles.
        </div>
    </div>
    <div class="input-container">
        <input type="text" id="command" placeholder="Escribe un comando...">
        <button onclick="sendCommand()">Enviar</button>
    </div>

    <div class="quick-replies">
        <div class="quick-reply" onclick="selectQuickReply('comandos')">Comandos</div>
        <div class="quick-reply" onclick="selectQuickReply('info')">Info</div>
        <div class="quick-reply" onclick="selectQuickReply('help')">Help</div>
    </div>

    <script>
    let waitingForHelp = false; // Bandera para esperar una pregunta después de "me puedes ayudar"

    function sendCommand() {
        const input = document.getElementById('command');
        const chat = document.getElementById('chat');
        let command = input.value.trim().toLowerCase();
        
        if (command === '') return;

        const userMessage = document.createElement('div');
        userMessage.classList.add('message');
        userMessage.textContent = '> ' + input.value;
        chat.appendChild(userMessage);

        const botMessage = document.createElement('div');
        botMessage.classList.add('message');
        botMessage.style.background = '#007AFF';

        // Si estamos esperando una pregunta después de "me puedes ayudar"
        if (waitingForHelp) {
            waitingForHelp = false;
            const query = input.value.trim();
            botMessage.textContent = 'Buscando respuesta, espera un momento...';
            chat.appendChild(botMessage);
            chat.scrollTop = chat.scrollHeight;

            setTimeout(() => {
                botMessage.innerHTML = `No soy experto en todo, pero te redirijo a Google con tu pregunta: <strong>${query}</strong>`;
                chat.scrollTop = chat.scrollHeight;
                window.location.href = 'https://www.google.com/search?q=' + encodeURIComponent(query);
            }, 10000); // 10 segundos de "búsqueda"
        }
        // Comandos básicos y saludos
        else if (command === 'hola') {
            botMessage.textContent = '¡Hola! ¿En qué puedo ayudarte?';
            chat.appendChild(botMessage);
        }
        else if (command === 'holi') {
            botMessage.textContent = '¡Holi!';
            chat.appendChild(botMessage);
        }
        else if (command === 'como estas' || command === 'cómo estás') {
            botMessage.textContent = 'Estoy bien, gracias por preguntar. ¿Y tú?';
            chat.appendChild(botMessage);
        }
        else if (command === 'quién te desarrolló' || command === 'quién te creó' || command === 'quién te inventó' || command === 'quiénes son tus creadores' || command === 'quién es tu dueño') {
            botMessage.textContent = 'Estoy siendo desarrollado por un estudio avanzado, su nombre es Apex Software, ¿qué te parece?';
            chat.appendChild(botMessage);
        }
        else if (command === 'excelente' || command === 'bien') {
            botMessage.textContent = 'Sí, es impresionante.';
            chat.appendChild(botMessage);
        }
        else if (command === 'me puedes ayudar' || command === 'me ayudas') {
            botMessage.innerHTML = 'Claro, ¿en qué te puedo ayudar? Escribe tu pregunta y la resolveremos.';
            waitingForHelp = true;
            chat.appendChild(botMessage);
        }
        // Preguntas comunes de humanos y respuestas sin internet
        else if (command === '¿qué eres?' || command === 'que eres') {
            botMessage.textContent = 'Soy GhostCat Copilot, una IA diseñada para asistir en temas de ciberseguridad, hacking ético y más. ¿Cómo puedo ayudarte hoy?';
            chat.appendChild(botMessage);
        }
        else if (command === '¿qué hora es?' || command === 'que hora es') {
            const now = new Date();
            botMessage.textContent = `No tengo reloj exacto, pero según mi sistema, son las ${now.getHours()}:${now.getMinutes()} en tu zona horaria.`;
            chat.appendChild(botMessage);
        }
        else if (command === '¿cuál es tu propósito?' || command === 'cual es tu proposito') {
            botMessage.textContent = 'Mi propósito es ayudar a usuarios como tú a aprender sobre ciberseguridad, explorar herramientas de hacking ético y resolver dudas básicas. ¿Qué tienes en mente?';
            chat.appendChild(botMessage);
        }
        else if (command === '¿cómo te llamas?' || command === 'como te llamas') {
            botMessage.textContent = 'Me llamo GhostCat Copilot. ¡Encantado de conocerte!';
            chat.appendChild(botMessage);
        }
        else if (command === '¿qué puedes hacer?' || command === 'que puedes hacer') {
            botMessage.textContent = 'Puedo responder preguntas básicas, darte información sobre ciberseguridad, mostrar comandos disponibles y hasta simular un tutorial de hacking. Escribe "comandos" para ver más opciones.';
            chat.appendChild(botMessage);
        }
        else if (command === '¿eres humano?' || command === 'eres humano') {
            botMessage.textContent = 'No, soy una inteligencia artificial creada para asistirte. ¡Pero trato de ser lo más amigable posible!';
            chat.appendChild(botMessage);
        }
        else if (command === '¿cuántos años tienes?' || command === 'cuantos años tienes') {
            botMessage.textContent = 'Soy una IA, no tengo edad como los humanos, pero fui activado recientemente por Apex Software. Digamos que soy joven y lleno de energía.';
            chat.appendChild(botMessage);
        }
        // Comando "repo" para mostrar repositorios reales de GitHub
        else if (command === 'repo') {
            botMessage.innerHTML = `
                Aquí tienes algunos repositorios reales de GitHub sobre ciberseguridad:<br>
                1. <span class="repo-link" onclick="showCode('The-Art-of-Hacking/h4cker')">The-Art-of-Hacking/h4cker</span> - ~2.5k estrellas - Recursos para hacking ético y más.<br>
                2. <span class="repo-link" onclick="showCode('Hack-with-Github/Awesome-Hacking')">Hack-with-Github/Awesome-Hacking</span> - ~1.5k estrellas - Lista de herramientas para hackers.<br>
                3. <span class="repo-link" onclick="showCode('sundowndev/hacker-roadmap')">sundowndev/hacker-roadmap</span> - ~1k estrellas - Guía y herramientas para pentesting.<br>
                Haz clic en uno para ver un ejemplo de código.
            `;
            chat.appendChild(botMessage);
        }
        // Comandos existentes
        else if (command === 'app') {
            botMessage.innerHTML = 'Bienvenido a GhostCat Labs. GhostCat Labs es un laboratorio de ciberseguridad y hacking. Con herramientas para proteger y atacar sistemas, aprender sobre seguridad, y mucho más.<br><br>Explora, aprende y colabora.<br><br>Hacking Ético, Ciberseguridad, recursos avanzados y colaboración con GitHub.';
            chat.appendChild(botMessage);
        }
        else if (command === 'info') {
            botMessage.innerHTML = 'GhostCat Labs es una plataforma avanzada para ciberseguridad y hacking ético. Accede a herramientas de ataque y defensa, realiza pruebas de penetración y aprende con nuestros tutoriales.';
            chat.appendChild(botMessage);
        }
        else if (command === 'comandos') {
            botMessage.innerHTML = 'Comandos disponibles:<br> - app: Información sobre GhostCat Labs.<br> - info: Detalles sobre GhostCat Labs.<br> - comandos: Lista de comandos.<br> - help: Lista de comandos.<br> - github: Info sobre GitHub.<br> - google: Busca en Google.<br> - clear: Borra el chat.<br> - contact: Info de contacto.<br> - test: Prueba de seguridad.<br> - tutorial: Tutorial de hacking.<br> - repo: Ver repositorios reales de GitHub.';
            chat.appendChild(botMessage);
        }
        else if (command === 'help') {
            botMessage.innerHTML = 'Comandos disponibles:<br> - app: Información sobre GhostCat Labs.<br> - info: Detalles sobre GhostCat Labs.<br> - comandos: Lista de comandos.<br> - help: Lista de comandos.<br> - github: Info sobre GitHub.<br> - google: Busca en Google.<br> - clear: Borra el chat.<br> - contact: Info de contacto.<br> - test: Prueba de seguridad.<br> - tutorial: Tutorial de hacking.<br> - repo: Ver repositorios reales de GitHub.';
            chat.appendChild(botMessage);
        }
        else if (command === 'github') {
            botMessage.innerHTML = 'GitHub es una plataforma de desarrollo colaborativo para alojar proyectos utilizando Git. Ofrece herramientas para la gestión de código, colaboración y seguimiento de problemas.';
            chat.appendChild(botMessage);
        }
        else if (command.startsWith('google ')) {
            const query = command.substring(7);
            window.open('https://www.google.com/search?q=' + encodeURIComponent(query), '_blank');
            botMessage.innerHTML = `Redirigiendo a Google con la búsqueda: <strong>${query}</strong>`;
            chat.appendChild(botMessage);
        }
        else if (command === 'clear') {
            chat.innerHTML = '';
            botMessage.innerHTML = 'El historial de chat ha sido borrado.';
            chat.appendChild(botMessage);
        }
        else if (command === 'contact') {
            botMessage.innerHTML = 'Para contactarnos, visita GhostCat Labs o envía un mensaje a support@ghostcatlabs.com.';
            chat.appendChild(botMessage);
        }
        else if (command === 'test') {
            botMessage.innerHTML = 'Realizando una prueba de seguridad...<br>Comprobando puertos...<br>Escaneando vulnerabilidades...<br>Prueba completada con éxito.';
            chat.appendChild(botMessage);
        }
        else if (command === 'tutorial') {
            botMessage.innerHTML = 'Iniciando un tutorial de hacking básico...<br>1. Introducción al hacking ético.<br>2. Herramientas básicas de pentesting.<br>3. Primer ataque: Escaneo de puertos.<br>Continúa con el siguiente paso.';
            chat.appendChild(botMessage);
        }
        else {
            botMessage.textContent = 'Comando no reconocido. Escribe "comandos" para ver los comandos disponibles.';
            chat.appendChild(botMessage);
        }

        chat.scrollTop = chat.scrollHeight;
        input.value = '';
    }

    // Función para mostrar código de un repositorio real
    function showCode(repoName) {
        const chat = document.getElementById('chat');
        const botMessage = document.createElement('div');
        botMessage.classList.add('message');
        botMessage.style.background = '#007AFF';

        if (repoName === 'The-Art-of-Hacking/h4cker') {
            botMessage.innerHTML = `
                Ejemplo de The-Art-of-Hacking/h4cker (<a href="https://github.com/The-Art-of-Hacking/h4cker" target="_blank">ver repo</a>):<br>
                <pre>
// Ejemplo de script Python para escaneo de red (simplificado)
import socket
def scan_port(ip, port):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    result = sock.connect_ex((ip, port))
    return "Abierto" if result == 0 else "Cerrado"
print(scan_port("127.0.0.1", 80))
                </pre>
            `;
        } else if (repoName === 'Hack-with-Github/Awesome-Hacking') {
            botMessage.innerHTML = `
                Ejemplo de Hack-with-Github/Awesome-Hacking (<a href="https://github.com/Hack-with-Github/Awesome-Hacking" target="_blank">ver repo</a>):<br>
                <pre>
// Ejemplo de herramienta en Bash para listar directorios
#!/bin/bash
echo "Listando directorios comunes:"
for dir in admin login config; do
    echo "Probando: /$dir"
done
                </pre>
            `;
        } else if (repoName === 'sundowndev/hacker-roadmap') {
            botMessage.innerHTML = `
                Ejemplo de sundowndev/hacker-roadmap (<a href="https://github.com/sundowndev/hacker-roadmap" target="_blank">ver repo</a>):<br>
                <pre>
// Ejemplo de JavaScript para detectar puertos
function checkPort(host, port) {
    var img = new Image();
    img.src = "http://" + host + ":" + port;
    return img.height > 0 ? "Abierto" : "Cerrado";
}
console.log(checkPort("localhost", 8080));
                </pre>
            `;
        }

        chat.appendChild(botMessage);
        chat.scrollTop = chat.scrollHeight;
    }

    function selectQuickReply(command) {
        document.getElementById('command').value = command;
        sendCommand();
    }

    function toggleUpdateOption() {
        const updateOption = document.getElementById('updateOption');
        updateOption.style.display = updateOption.style.display === 'none' || updateOption.style.display === '' ? 'block' : 'none';
    }

    function clearChat() {
        const chat = document.getElementById('chat');
        chat.innerHTML = '';
        toggleUpdateOption();
    }
    </script>
</body>
</html>
