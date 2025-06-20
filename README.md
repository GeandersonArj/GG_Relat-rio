<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Relatório Técnico</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom styles for better aesthetics and mobile responsiveness */
        body {
            font-family: "Inter", sans-serif;
            background-color: #f0f4f8; /* Light blue-gray background */
            display: flex;
            justify-content: center;
            align-items: flex-start; /* Align items to the top for better scrolling on mobile */
            min-height: 100vh;
            padding: 20px;
            box-sizing: border-box;
        }
        .container {
            background-color: #ffffff;
            border-radius: 16px; /* More rounded corners */
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1); /* Softer shadow */
            padding: 24px;
            width: 100%;
            max-width: 800px; /* Max width for desktop */
            display: flex;
            flex-direction: column;
            gap: 20px;
        }
        h1, h2 {
            color: #1a202c; /* Darker text for headings */
        }
        input[type="text"], textarea {
            padding: 12px 16px;
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            outline: none;
            transition: border-color 0.3s ease;
            width: 100%;
            box-sizing: border-box; /* Include padding and border in the element's total width and height */
        }
        input[type="text"]:focus, textarea:focus {
            border-color: #3b82f6; /* Blue focus border */
        }
        button {
            background-color: #3b82f6; /* Blue button */
            color: white;
            padding: 12px 20px;
            border-radius: 12px;
            transition: background-color 0.3s ease, transform 0.1s ease;
            cursor: pointer;
            flex-grow: 1; /* Allow buttons to grow */
        }
        button:hover {
            background-color: #2563eb; /* Darker blue on hover */
            transform: translateY(-1px); /* Slight lift effect */
        }
        .section-card {
            background-color: #f8fafc; /* Lighter background for sections */
            padding: 16px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05); /* Subtle shadow for sections */
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        .camera-controls {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            margin-top: 10px;
        }
        .camera-buttons-row {
            display: flex;
            gap: 10px;
            width: 100%;
            justify-content: space-between;
        }
        .camera-controls video {
            width: 100%;
            max-width: 400px; /* Max width for video stream */
            height: auto;
            border-radius: 8px;
            background-color: #000;
        }
        .camera-controls canvas {
            display: none; /* Canvas is hidden, used for capturing */
        }
        .photo-preview {
            width: 100%;
            max-width: 300px; /* Max width for preview image */
            height: auto;
            border-radius: 8px;
            margin-top: 8px;
            border: 1px solid #e2e8f0;
            object-fit: cover;
        }
        .photo-comment {
            width: 100%;
            padding: 8px 12px;
            border: 1px solid #cbd5e0;
            border-radius: 8px;
            font-size: 0.9em;
            margin-top: 5px;
            resize: vertical; /* Allow vertical resizing */
        }
        .message-box {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #333;
            color: white;
            padding: 15px 25px;
            border-radius: 8px;
            z-index: 1000;
            display: none;
            opacity: 0;
            transition: opacity 0.3s ease-in-out;
            text-align: center;
        }
        .message-box.show {
            display: block;
            opacity: 1;
        }

        /* Styles for printing */
        @media print {
            body {
                background-color: #fff;
                padding: 0;
                margin: 0;
                display: block;
            }
            .container {
                box-shadow: none;
                border-radius: 0;
                padding: 0;
                max-width: none;
                width: auto;
            }
            .no-print {
                display: none !important;
            }
            .section-card {
                box-shadow: none;
                border: 1px solid #e2e8f0;
                margin-bottom: 15px;
                page-break-inside: avoid; /* Prevent breaking inside a section */
            }
            .photo-preview {
                max-width: 100%; /* Ensure images fit within print area */
            }
            .photo-comment {
                border: none; /* No border for comments in print */
                padding: 0;
                margin-top: 2px;
                font-size: 0.8em;
            }
        }

        /* Responsive adjustments */
        @media (max-width: 640px) {
            body {
                padding: 10px;
                align-items: flex-start;
            }
            .container {
                padding: 16px;
                gap: 15px;
            }
            button {
                width: 100%;
            }
            .camera-buttons-row {
                flex-direction: column;
                gap: 5px;
            }
            .camera-controls video {
                max-width: 100%;
            }
            .photo-preview {
                max-width: 100%;
            }
        }
    </style>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen p-4">
    <div class="container">
        <h1 class="text-3xl font-bold text-center text-gray-800 mb-4 no-print">Relatório Técnico</h1>

        <div class="section-card">
            <h2 class="text-2xl font-semibold text-gray-700"> Relatório de serviço</h2>
            <label for="reportTitle" class="text-gray-600">Título do Relatório:</label>
            <input type="text" id="reportTitle" placeholder="Ex: Relatório de serviço de Equipamento X" class="text-xl">

            <label for="reportDate" class="text-gray-600">Data:</label>
            <input type="date" id="reportDate" class="text-base">

            <label for="reportAuthor" class="text-gray-600">Autor:</label>
            <input type="text" id="reportAuthor" placeholder="Seu Nome" class="text-base">
        </div>

        <div class="section-card">
            <h2 class="text-2xl font-semibold text-gray-700">1. Descrição do Problema/Sintomas</h2>
            <textarea id="problemDescription" rows="5" placeholder="Descreva o problema ou a situação atual..." class="text-base"></textarea>
            <div class="camera-controls no-print">
                <div class="camera-buttons-row">
                    <button class="take-photo-button" data-target-video="videoProblem" data-target-canvas="canvasProblem" data-target-img="imgProblem" data-target-comment="commentProblem">Tirar Foto</button>
                    <input type="file" id="fileInputProblem" accept="image/*" class="hidden" data-target-img="imgProblem" data-target-comment="commentProblem">
                    <button class="add-file-button bg-gray-500 hover:bg-gray-600" data-target-input="fileInputProblem">Adicionar Arquivo</button>
                </div>
                <video id="videoProblem" autoplay playsinline class="rounded-lg shadow-md hidden"></video>
                <canvas id="canvasProblem" class="hidden"></canvas>
                <button class="capture-button bg-green-500 text-white p-2 rounded-lg hover:bg-green-600 transition-all duration-200 hidden" data-video-id="videoProblem" data-canvas-id="canvasProblem" data-img-id="imgProblem" data-comment-id="commentProblem">Capturar Foto</button>
            </div>
            <img id="imgProblem" class="photo-preview hidden" alt="Foto da Descrição do Problema" src="https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto">
            <textarea id="commentProblem" class="photo-comment hidden" rows="2" placeholder="Adicionar comentário para esta foto..."></textarea>
        </div>

        <div class="section-card">
            <h2 class="text-2xl font-semibold text-gray-700">2. Ações Realizadas/Solução Proposta</h2>
            <textarea id="solutionProposed" rows="5" placeholder="Descreva as ações realizadas ou a solução proposta..." class="text-base"></textarea>

            <div class="camera-controls no-print">
                <div class="camera-buttons-row">
                    <button class="take-photo-button" data-target-video="videoSolution" data-target-canvas="canvasSolution" data-target-img="imgSolution" data-target-comment="commentSolution">Tirar Foto 1</button>
                    <input type="file" id="fileInputSolution" accept="image/*" class="hidden" data-target-img="imgSolution" data-target-comment="commentSolution">
                    <button class="add-file-button bg-gray-500 hover:bg-gray-600" data-target-input="fileInputSolution">Adicionar Arquivo 1</button>
                </div>
                <video id="videoSolution" autoplay playsinline class="rounded-lg shadow-md hidden"></video>
                <canvas id="canvasSolution" class="hidden"></canvas>
                <button class="capture-button bg-green-500 text-white p-2 rounded-lg hover:bg-green-600 transition-all duration-200 hidden" data-video-id="videoSolution" data-canvas-id="canvasSolution" data-img-id="imgSolution" data-comment-id="commentSolution">Capturar Foto</button>
            </div>
            <img id="imgSolution" class="photo-preview hidden" alt="Foto da Solução Proposta" src="https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto">
            <textarea id="commentSolution" class="photo-comment hidden" rows="2" placeholder="Adicionar comentário para esta foto..."></textarea>

            <div class="camera-controls no-print">
                <div class="camera-buttons-row">
                    <button class="take-photo-button" data-target-video="videoSolution2" data-target-canvas="canvasSolution2" data-target-img="imgSolution2" data-target-comment="commentSolution2">Tirar Foto 2</button>
                    <input type="file" id="fileInputSolution2" accept="image/*" class="hidden" data-target-img="imgSolution2" data-target-comment="commentSolution2">
                    <button class="add-file-button bg-gray-500 hover:bg-gray-600" data-target-input="fileInputSolution2">Adicionar Arquivo 2</button>
                </div>
                <video id="videoSolution2" autoplay playsinline class="rounded-lg shadow-md hidden"></video>
                <canvas id="canvasSolution2" class="hidden"></canvas>
                <button class="capture-button bg-green-500 text-white p-2 rounded-lg hover:bg-green-600 transition-all duration-200 hidden" data-video-id="videoSolution2" data-canvas-id="canvasSolution2" data-img-id="imgSolution2" data-comment-id="commentSolution2">Capturar Foto</button>
            </div>
            <img id="imgSolution2" class="photo-preview hidden" alt="Foto da Solução Proposta 2" src="https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto">
            <textarea id="commentSolution2" class="photo-comment hidden" rows="2" placeholder="Adicionar comentário para esta foto..."></textarea>

            <div class="camera-controls no-print">
                <div class="camera-buttons-row">
                    <button class="take-photo-button" data-target-video="videoSolution3" data-target-canvas="canvasSolution3" data-target-img="imgSolution3" data-target-comment="commentSolution3">Tirar Foto 3</button>
                    <input type="file" id="fileInputSolution3" accept="image/*" class="hidden" data-target-img="imgSolution3" data-target-comment="commentSolution3">
                    <button class="add-file-button bg-gray-500 hover:bg-gray-600" data-target-input="fileInputSolution3">Adicionar Arquivo 3</button>
                </div>
                <video id="videoSolution3" autoplay playsinline class="rounded-lg shadow-md hidden"></video>
                <canvas id="canvasSolution3" class="hidden"></canvas>
                <button class="capture-button bg-green-500 text-white p-2 rounded-lg hover:bg-green-600 transition-all duration-200 hidden" data-video-id="videoSolution3" data-canvas-id="canvasSolution3" data-img-id="imgSolution3" data-comment-id="commentSolution3">Capturar Foto</button>
            </div>
            <img id="imgSolution3" class="photo-preview hidden" alt="Foto da Solução Proposta 3" src="https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto">
            <textarea id="commentSolution3" class="photo-comment hidden" rows="2" placeholder="Adicionar comentário para esta foto..."></textarea>
        </div>

        <div class="section-card">
            <h2 class="text-2xl font-semibold text-gray-700">3. Observações Adicionais/Executantes</h2>
            <textarea id="additionalObservations" rows="5" placeholder="Adicione quaisquer observações relevantes..." class="text-base"></textarea>
            <div class="camera-controls no-print">
                <div class="camera-buttons-row">
                    <button class="take-photo-button" data-target-video="videoObservations" data-target-canvas="canvasObservations" data-target-img="imgObservations" data-target-comment="commentObservations">Tirar Foto</button>
                    <input type="file" id="fileInputObservations" accept="image/*" class="hidden" data-target-img="imgObservations" data-target-comment="commentObservations">
                    <button class="add-file-button bg-gray-500 hover:bg-gray-600" data-target-input="fileInputObservations">Adicionar Arquivo</button>
                </div>
                <video id="videoObservations" autoplay playsinline class="rounded-lg shadow-md hidden"></video>
                <canvas id="canvasObservations" class="hidden"></canvas>
                <button class="capture-button bg-green-500 text-white p-2 rounded-lg hover:bg-green-600 transition-all duration-200 hidden" data-video-id="videoObservations" data-canvas-id="canvasObservations" data-img-id="imgObservations" data-comment-id="commentObservations">Capturar Foto</button>
            </div>
            <img id="imgObservations" class="photo-preview hidden" alt="Foto de Observações Adicionais" src="https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto">
            <textarea id="commentObservations" class="photo-comment hidden" rows="2" placeholder="Adicionar comentário para esta foto..."></textarea>
        </div>

        <button id="generateReportButton" class="bg-blue-500 text-white p-3 rounded-xl hover:bg-blue-600 transition-all duration-300 shadow-md mt-4 no-print">Gerar Relatório (Imprimir/PDF)</button>
    </div>

    <div id="messageBox" class="message-box"></div>

    <script>
        let currentStream = null; // To hold the active camera stream
        let activeVideoElement = null; // To track the currently active video element
        let activeCanvasElement = null; // To track the currently active canvas element
        let activeImageElement = null; // To track the currently active image element
        let activeCaptureButton = null; // To track the currently active capture button
        let activeCommentElement = null; // To track the currently active comment element

        const messageBox = document.getElementById('messageBox');

        /**
         * Displays a temporary message box with the given text.
         * @param {string} message - The message to display.
         * @param {number} duration - The duration in milliseconds to show the message.
         */
        function showMessageBox(message, duration = 3000) {
            messageBox.textContent = message;
            messageBox.classList.add('show');
            setTimeout(() => {
                messageBox.classList.remove('show');
            }, duration);
        }

        /**
         * Stops the current camera stream if active and hides associated elements.
         */
        function stopCameraStream() {
            if (currentStream) {
                currentStream.getTracks().forEach(track => track.stop());
                currentStream = null;
            }
            if (activeVideoElement) {
                activeVideoElement.style.display = 'none';
                activeVideoElement.srcObject = null;
                activeVideoElement = null;
            }
            if (activeCaptureButton) {
                activeCaptureButton.style.display = 'none';
                activeCaptureButton = null;
            }
            // Reset active elements related to camera/file input
            activeCanvasElement = null;
            activeImageElement = null;
            activeCommentElement = null;
        }

        /**
         * Initializes camera access for a specific section.
         * @param {HTMLVideoElement} videoElement - The video element to display the stream.
         * @param {HTMLCanvasElement} canvasElement - The canvas element for capturing.
         * @param {HTMLImageElement} imageElement - The image element to display the captured photo.
         * @param {HTMLButtonElement} captureButton - The capture button for this section.
         * @param {HTMLTextAreaElement} commentElement - The textarea element for comments.
         */
        async function startCamera(videoElement, canvasElement, imageElement, captureButton, commentElement) {
            // Stop any previously active camera stream or file input view
            stopCameraStream();

            activeVideoElement = videoElement;
            activeCanvasElement = canvasElement;
            activeImageElement = imageElement;
            activeCaptureButton = captureButton;
            activeCommentElement = commentElement; // Set active comment element

            // Hide existing image and comment if starting camera
            activeImageElement.style.display = 'none';
            activeCommentElement.style.display = 'none';

            activeVideoElement.style.display = 'block'; // Show video element
            activeCaptureButton.style.display = 'block'; // Show capture button

            const constraints = {
                video: {
                    // Try to get the environment (rear) camera first
                    facingMode: { exact: "environment" }
                }
            };

            try {
                // Attempt to get the environment camera
                currentStream = await navigator.mediaDevices.getUserMedia(constraints);
                activeVideoElement.srcObject = currentStream;
                activeVideoElement.play();
            } catch (err) {
                console.warn("Failed to get environment camera, trying user camera:", err);
                // If environment camera fails, try the user (front) camera
                constraints.video.facingMode = "user";
                try {
                    currentStream = await navigator.mediaDevices.getUserMedia(constraints);
                    activeVideoElement.srcObject = currentStream;
                    activeVideoElement.play();
                } catch (userErr) {
                    console.error("Error accessing any camera: ", userErr);
                    showMessageBox('Não foi possível acessar a câmera. Verifique as permissões.');
                    activeVideoElement.style.display = 'none';
                    activeCaptureButton.style.display = 'none';
                    stopCameraStream(); // Ensure all related elements are hidden
                }
            }
        }

        // Event listeners for "Tirar Foto" buttons
        document.querySelectorAll('.take-photo-button').forEach(button => {
            button.addEventListener('click', (event) => {
                const targetVideoId = event.target.dataset.targetVideo;
                const targetCanvasId = event.target.dataset.targetCanvas;
                const targetImgId = event.target.dataset.targetImg;
                const targetCommentId = event.target.dataset.targetComment;

                const video = document.getElementById(targetVideoId);
                const canvas = document.getElementById(targetCanvasId);
                const img = document.getElementById(targetImgId);
                const captureBtn = document.querySelector(`.capture-button[data-video-id="${targetVideoId}"]`);
                const comment = document.getElementById(targetCommentId);

                startCamera(video, canvas, img, captureBtn, comment);
            });
        });

        // Event listeners for "Capturar Foto" buttons
        document.querySelectorAll('.capture-button').forEach(button => {
            button.addEventListener('click', (event) => {
                if (activeVideoElement && currentStream) {
                    const context = activeCanvasElement.getContext('2d');
                    activeCanvasElement.width = activeVideoElement.videoWidth;
                    activeCanvasElement.height = activeVideoElement.videoHeight;
                    context.drawImage(activeVideoElement, 0, 0, activeCanvasElement.width, activeCanvasElement.height);

                    const imageDataURL = activeCanvasElement.toDataURL('image/png');
                    activeImageElement.src = imageDataURL;
                    activeImageElement.style.display = 'block'; // Show the image
                    activeCommentElement.style.display = 'block'; // Show the comment textarea

                    stopCameraStream(); // Stop camera after capturing
                    showMessageBox('Foto capturada!');
                } else {
                    showMessageBox('Nenhuma câmera ativa para capturar.');
                }
            });
        });

        // Event listeners for "Adicionar Arquivo" buttons
        document.querySelectorAll('.add-file-button').forEach(button => {
            button.addEventListener('click', (event) => {
                const targetInputId = event.target.dataset.targetInput;
                const fileInput = document.getElementById(targetInputId);
                if (fileInput) {
                    stopCameraStream(); // Stop camera if active
                    fileInput.click(); // Programmatically click the hidden file input
                }
            });
        });

        // Event listeners for hidden file inputs
        document.querySelectorAll('input[type="file"]').forEach(input => {
            input.addEventListener('change', (event) => {
                const file = event.target.files[0];
                if (file) {
                    const targetImgId = event.target.dataset.targetImg;
                    const targetCommentId = event.target.dataset.targetComment;
                    const imgElement = document.getElementById(targetImgId);
                    const commentElement = document.getElementById(targetCommentId);

                    const reader = new FileReader();
                    reader.onload = (e) => {
                        if (imgElement) {
                            imgElement.src = e.target.result;
                            imgElement.style.display = 'block'; // Show the image
                        }
                        if (commentElement) {
                            commentElement.style.display = 'block'; // Show the comment textarea
                        }
                        showMessageBox('Arquivo adicionado!');
                    };
                    reader.readAsDataURL(file);
                } else {
                    showMessageBox('Nenhum arquivo selecionado.');
                }
            });
        });

        // Event listener for Generate Report button
        document.getElementById('generateReportButton').addEventListener('click', () => {
            stopCameraStream(); // Ensure camera is off before printing
            window.print(); // Triggers the browser's print dialog
        });

        // Set current date on load
        document.addEventListener('DOMContentLoaded', () => {
            const today = new Date();
            const yyyy = today.getFullYear();
            const mm = String(today.getMonth() + 1).padStart(2, '0'); // Months are 0-indexed
            const dd = String(today.getDate()).padStart(2, '0');
            document.getElementById('reportDate').value = `${yyyy}-${mm}-${dd}`;
        });
    </script>
</body>
</html># GG_Relat-rio
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Relatório Técnico</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom styles for better aesthetics and mobile responsiveness */
        body {
            font-family: "Inter", sans-serif;
            background-color: #f0f4f8; /* Light blue-gray background */
            display: flex;
            justify-content: center;
            align-items: flex-start; /* Align items to the top for better scrolling on mobile */
            min-height: 100vh;
            padding: 20px;
            box-sizing: border-box;
        }
        .container {
            background-color: #ffffff;
            border-radius: 16px; /* More rounded corners */
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1); /* Softer shadow */
            padding: 24px;
            width: 100%;
            max-width: 800px; /* Max width for desktop */
            display: flex;
            flex-direction: column;
            gap: 20px;
        }
        h1, h2 {
            color: #1a202c; /* Darker text for headings */
        }
        input[type="text"], textarea {
            padding: 12px 16px;
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            outline: none;
            transition: border-color 0.3s ease;
            width: 100%;
            box-sizing: border-box; /* Include padding and border in the element's total width and height */
        }
        input[type="text"]:focus, textarea:focus {
            border-color: #3b82f6; /* Blue focus border */
        }
        button {
            background-color: #3b82f6; /* Blue button */
            color: white;
            padding: 12px 20px;
            border-radius: 12px;
            transition: background-color 0.3s ease, transform 0.1s ease;
            cursor: pointer;
            flex-grow: 1; /* Allow buttons to grow */
        }
        button:hover {
            background-color: #2563eb; /* Darker blue on hover */
            transform: translateY(-1px); /* Slight lift effect */
        }
        .section-card {
            background-color: #f8fafc; /* Lighter background for sections */
            padding: 16px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05); /* Subtle shadow for sections */
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        .camera-controls {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            margin-top: 10px;
        }
        .camera-buttons-row {
            display: flex;
            gap: 10px;
            width: 100%;
            justify-content: space-between;
        }
        .camera-controls video {
            width: 100%;
            max-width: 400px; /* Max width for video stream */
            height: auto;
            border-radius: 8px;
            background-color: #000;
        }
        .camera-controls canvas {
            display: none; /* Canvas is hidden, used for capturing */
        }
        .photo-preview {
            width: 100%;
            max-width: 300px; /* Max width for preview image */
            height: auto;
            border-radius: 8px;
            margin-top: 8px;
            border: 1px solid #e2e8f0;
            object-fit: cover;
        }
        .photo-comment {
            width: 100%;
            padding: 8px 12px;
            border: 1px solid #cbd5e0;
            border-radius: 8px;
            font-size: 0.9em;
            margin-top: 5px;
            resize: vertical; /* Allow vertical resizing */
        }
        .message-box {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #333;
            color: white;
            padding: 15px 25px;
            border-radius: 8px;
            z-index: 1000;
            display: none;
            opacity: 0;
            transition: opacity 0.3s ease-in-out;
            text-align: center;
        }
        .message-box.show {
            display: block;
            opacity: 1;
        }

        /* Styles for printing */
        @media print {
            body {
                background-color: #fff;
                padding: 0;
                margin: 0;
                display: block;
            }
            .container {
                box-shadow: none;
                border-radius: 0;
                padding: 0;
                max-width: none;
                width: auto;
            }
            .no-print {
                display: none !important;
            }
            .section-card {
                box-shadow: none;
                border: 1px solid #e2e8f0;
                margin-bottom: 15px;
                page-break-inside: avoid; /* Prevent breaking inside a section */
            }
            .photo-preview {
                max-width: 100%; /* Ensure images fit within print area */
            }
            .photo-comment {
                border: none; /* No border for comments in print */
                padding: 0;
                margin-top: 2px;
                font-size: 0.8em;
            }
        }

        /* Responsive adjustments */
        @media (max-width: 640px) {
            body {
                padding: 10px;
                align-items: flex-start;
            }
            .container {
                padding: 16px;
                gap: 15px;
            }
            button {
                width: 100%;
            }
            .camera-buttons-row {
                flex-direction: column;
                gap: 5px;
            }
            .camera-controls video {
                max-width: 100%;
            }
            .photo-preview {
                max-width: 100%;
            }
        }
    </style>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen p-4">
    <div class="container">
        <h1 class="text-3xl font-bold text-center text-gray-800 mb-4 no-print">Relatório Técnico</h1>

        <div class="section-card">
            <h2 class="text-2xl font-semibold text-gray-700"> Relatório de serviço</h2>
            <label for="reportTitle" class="text-gray-600">Título do Relatório:</label>
            <input type="text" id="reportTitle" placeholder="Ex: Relatório de serviço de Equipamento X" class="text-xl">

            <label for="reportDate" class="text-gray-600">Data:</label>
            <input type="date" id="reportDate" class="text-base">

            <label for="reportAuthor" class="text-gray-600">Autor:</label>
            <input type="text" id="reportAuthor" placeholder="Seu Nome" class="text-base">
        </div>

        <div class="section-card">
            <h2 class="text-2xl font-semibold text-gray-700">1. Descrição do Problema/Sintomas</h2>
            <textarea id="problemDescription" rows="5" placeholder="Descreva o problema ou a situação atual..." class="text-base"></textarea>
            <div class="camera-controls no-print">
                <div class="camera-buttons-row">
                    <button class="take-photo-button" data-target-video="videoProblem" data-target-canvas="canvasProblem" data-target-img="imgProblem" data-target-comment="commentProblem">Tirar Foto</button>
                    <input type="file" id="fileInputProblem" accept="image/*" class="hidden" data-target-img="imgProblem" data-target-comment="commentProblem">
                    <button class="add-file-button bg-gray-500 hover:bg-gray-600" data-target-input="fileInputProblem">Adicionar Arquivo</button>
                </div>
                <video id="videoProblem" autoplay playsinline class="rounded-lg shadow-md hidden"></video>
                <canvas id="canvasProblem" class="hidden"></canvas>
                <button class="capture-button bg-green-500 text-white p-2 rounded-lg hover:bg-green-600 transition-all duration-200 hidden" data-video-id="videoProblem" data-canvas-id="canvasProblem" data-img-id="imgProblem" data-comment-id="commentProblem">Capturar Foto</button>
            </div>
            <img id="imgProblem" class="photo-preview hidden" alt="Foto da Descrição do Problema" src="https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto">
            <textarea id="commentProblem" class="photo-comment hidden" rows="2" placeholder="Adicionar comentário para esta foto..."></textarea>
        </div>

        <div class="section-card">
            <h2 class="text-2xl font-semibold text-gray-700">2. Ações Realizadas/Solução Proposta</h2>
            <textarea id="solutionProposed" rows="5" placeholder="Descreva as ações realizadas ou a solução proposta..." class="text-base"></textarea>

            <div class="camera-controls no-print">
                <div class="camera-buttons-row">
                    <button class="take-photo-button" data-target-video="videoSolution" data-target-canvas="canvasSolution" data-target-img="imgSolution" data-target-comment="commentSolution">Tirar Foto 1</button>
                    <input type="file" id="fileInputSolution" accept="image/*" class="hidden" data-target-img="imgSolution" data-target-comment="commentSolution">
                    <button class="add-file-button bg-gray-500 hover:bg-gray-600" data-target-input="fileInputSolution">Adicionar Arquivo 1</button>
                </div>
                <video id="videoSolution" autoplay playsinline class="rounded-lg shadow-md hidden"></video>
                <canvas id="canvasSolution" class="hidden"></canvas>
                <button class="capture-button bg-green-500 text-white p-2 rounded-lg hover:bg-green-600 transition-all duration-200 hidden" data-video-id="videoSolution" data-canvas-id="canvasSolution" data-img-id="imgSolution" data-comment-id="commentSolution">Capturar Foto</button>
            </div>
            <img id="imgSolution" class="photo-preview hidden" alt="Foto da Solução Proposta" src="https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto">
            <textarea id="commentSolution" class="photo-comment hidden" rows="2" placeholder="Adicionar comentário para esta foto..."></textarea>

            <div class="camera-controls no-print">
                <div class="camera-buttons-row">
                    <button class="take-photo-button" data-target-video="videoSolution2" data-target-canvas="canvasSolution2" data-target-img="imgSolution2" data-target-comment="commentSolution2">Tirar Foto 2</button>
                    <input type="file" id="fileInputSolution2" accept="image/*" class="hidden" data-target-img="imgSolution2" data-target-comment="commentSolution2">
                    <button class="add-file-button bg-gray-500 hover:bg-gray-600" data-target-input="fileInputSolution2">Adicionar Arquivo 2</button>
                </div>
                <video id="videoSolution2" autoplay playsinline class="rounded-lg shadow-md hidden"></video>
                <canvas id="canvasSolution2" class="hidden"></canvas>
                <button class="capture-button bg-green-500 text-white p-2 rounded-lg hover:bg-green-600 transition-all duration-200 hidden" data-video-id="videoSolution2" data-canvas-id="canvasSolution2" data-img-id="imgSolution2" data-comment-id="commentSolution2">Capturar Foto</button>
            </div>
            <img id="imgSolution2" class="photo-preview hidden" alt="Foto da Solução Proposta 2" src="https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto">
            <textarea id="commentSolution2" class="photo-comment hidden" rows="2" placeholder="Adicionar comentário para esta foto..."></textarea>

            <div class="camera-controls no-print">
                <div class="camera-buttons-row">
                    <button class="take-photo-button" data-target-video="videoSolution3" data-target-canvas="canvasSolution3" data-target-img="imgSolution3" data-target-comment="commentSolution3">Tirar Foto 3</button>
                    <input type="file" id="fileInputSolution3" accept="image/*" class="hidden" data-target-img="imgSolution3" data-target-comment="commentSolution3">
                    <button class="add-file-button bg-gray-500 hover:bg-gray-600" data-target-input="fileInputSolution3">Adicionar Arquivo 3</button>
                </div>
                <video id="videoSolution3" autoplay playsinline class="rounded-lg shadow-md hidden"></video>
                <canvas id="canvasSolution3" class="hidden"></canvas>
                <button class="capture-button bg-green-500 text-white p-2 rounded-lg hover:bg-green-600 transition-all duration-200 hidden" data-video-id="videoSolution3" data-canvas-id="canvasSolution3" data-img-id="imgSolution3" data-comment-id="commentSolution3">Capturar Foto</button>
            </div>
            <img id="imgSolution3" class="photo-preview hidden" alt="Foto da Solução Proposta 3" src="https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto">
            <textarea id="commentSolution3" class="photo-comment hidden" rows="2" placeholder="Adicionar comentário para esta foto..."></textarea>
        </div>

        <div class="section-card">
            <h2 class="text-2xl font-semibold text-gray-700">3. Observações Adicionais/Executantes</h2>
            <textarea id="additionalObservations" rows="5" placeholder="Adicione quaisquer observações relevantes..." class="text-base"></textarea>
            <div class="camera-controls no-print">
                <div class="camera-buttons-row">
                    <button class="take-photo-button" data-target-video="videoObservations" data-target-canvas="canvasObservations" data-target-img="imgObservations" data-target-comment="commentObservations">Tirar Foto</button>
                    <input type="file" id="fileInputObservations" accept="image/*" class="hidden" data-target-img="imgObservations" data-target-comment="commentObservations">
                    <button class="add-file-button bg-gray-500 hover:bg-gray-600" data-target-input="fileInputObservations">Adicionar Arquivo</button>
                </div>
                <video id="videoObservations" autoplay playsinline class="rounded-lg shadow-md hidden"></video>
                <canvas id="canvasObservations" class="hidden"></canvas>
                <button class="capture-button bg-green-500 text-white p-2 rounded-lg hover:bg-green-600 transition-all duration-200 hidden" data-video-id="videoObservations" data-canvas-id="canvasObservations" data-img-id="imgObservations" data-comment-id="commentObservations">Capturar Foto</button>
            </div>
            <img id="imgObservations" class="photo-preview hidden" alt="Foto de Observações Adicionais" src="https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto">
            <textarea id="commentObservations" class="photo-comment hidden" rows="2" placeholder="Adicionar comentário para esta foto..."></textarea>
        </div>

        <button id="generateReportButton" class="bg-blue-500 text-white p-3 rounded-xl hover:bg-blue-600 transition-all duration-300 shadow-md mt-4 no-print">Gerar Relatório (Imprimir/PDF)</button>
    </div>

    <div id="messageBox" class="message-box"></div>

    <script>
        let currentStream = null; // To hold the active camera stream
        let activeVideoElement = null; // To track the currently active video element
        let activeCanvasElement = null; // To track the currently active canvas element
        let activeImageElement = null; // To track the currently active image element
        let activeCaptureButton = null; // To track the currently active capture button
        let activeCommentElement = null; // To track the currently active comment element

        const messageBox = document.getElementById('messageBox');

        /**
         * Displays a temporary message box with the given text.
         * @param {string} message - The message to display.
         * @param {number} duration - The duration in milliseconds to show the message.
         */
        function showMessageBox(message, duration = 3000) {
            messageBox.textContent = message;
            messageBox.classList.add('show');
            setTimeout(() => {
                messageBox.classList.remove('show');
            }, duration);
        }

        /**
         * Stops the current camera stream if active and hides associated elements.
         */
        function stopCameraStream() {
            if (currentStream) {
                currentStream.getTracks().forEach(track => track.stop());
                currentStream = null;
            }
            if (activeVideoElement) {
                activeVideoElement.style.display = 'none';
                activeVideoElement.srcObject = null;
                activeVideoElement = null;
            }
            if (activeCaptureButton) {
                activeCaptureButton.style.display = 'none';
                activeCaptureButton = null;
            }
            // Reset active elements related to camera/file input
            activeCanvasElement = null;
            activeImageElement = null;
            activeCommentElement = null;
        }

        /**
         * Initializes camera access for a specific section.
         * @param {HTMLVideoElement} videoElement - The video element to display the stream.
         * @param {HTMLCanvasElement} canvasElement - The canvas element for capturing.
         * @param {HTMLImageElement} imageElement - The image element to display the captured photo.
         * @param {HTMLButtonElement} captureButton - The capture button for this section.
         * @param {HTMLTextAreaElement} commentElement - The textarea element for comments.
         */
        async function startCamera(videoElement, canvasElement, imageElement, captureButton, commentElement) {
            // Stop any previously active camera stream or file input view
            stopCameraStream();

            activeVideoElement = videoElement;
            activeCanvasElement = canvasElement;
            activeImageElement = imageElement;
            activeCaptureButton = captureButton;
            activeCommentElement = commentElement; // Set active comment element

            // Hide existing image and comment if starting camera
            activeImageElement.style.display = 'none';
            activeCommentElement.style.display = 'none';

            activeVideoElement.style.display = 'block'; // Show video element
            activeCaptureButton.style.display = 'block'; // Show capture button

            const constraints = {
                video: {
                    // Try to get the environment (rear) camera first
                    facingMode: { exact: "environment" }
                }
            };

            try {
                // Attempt to get the environment camera
                currentStream = await navigator.mediaDevices.getUserMedia(constraints);
                activeVideoElement.srcObject = currentStream;
                activeVideoElement.play();
            } catch (err) {
                console.warn("Failed to get environment camera, trying user camera:", err);
                // If environment camera fails, try the user (front) camera
                constraints.video.facingMode = "user";
                try {
                    currentStream = await navigator.mediaDevices.getUserMedia(constraints);
                    activeVideoElement.srcObject = currentStream;
                    activeVideoElement.play();
                } catch (userErr) {
                    console.error("Error accessing any camera: ", userErr);
                    showMessageBox('Não foi possível acessar a câmera. Verifique as permissões.');
                    activeVideoElement.style.display = 'none';
                    activeCaptureButton.style.display = 'none';
                    stopCameraStream(); // Ensure all related elements are hidden
                }
            }
        }

        // Event listeners for "Tirar Foto" buttons
        document.querySelectorAll('.take-photo-button').forEach(button => {
            button.addEventListener('click', (event) => {
                const targetVideoId = event.target.dataset.targetVideo;
                const targetCanvasId = event.target.dataset.targetCanvas;
                const targetImgId = event.target.dataset.targetImg;
                const targetCommentId = event.target.dataset.targetComment;

                const video = document.getElementById(targetVideoId);
                const canvas = document.getElementById(targetCanvasId);
                const img = document.getElementById(targetImgId);
                const captureBtn = document.querySelector(`.capture-button[data-video-id="${targetVideoId}"]`);
                const comment = document.getElementById(targetCommentId);

                startCamera(video, canvas, img, captureBtn, comment);
            });
        });

        // Event listeners for "Capturar Foto" buttons
        document.querySelectorAll('.capture-button').forEach(button => {
            button.addEventListener('click', (event) => {
                if (activeVideoElement && currentStream) {
                    const context = activeCanvasElement.getContext('2d');
                    activeCanvasElement.width = activeVideoElement.videoWidth;
                    activeCanvasElement.height = activeVideoElement.videoHeight;
                    context.drawImage(activeVideoElement, 0, 0, activeCanvasElement.width, activeCanvasElement.height);

                    const imageDataURL = activeCanvasElement.toDataURL('image/png');
                    activeImageElement.src = imageDataURL;
                    activeImageElement.style.display = 'block'; // Show the image
                    activeCommentElement.style.display = 'block'; // Show the comment textarea

                    stopCameraStream(); // Stop camera after capturing
                    showMessageBox('Foto capturada!');
                } else {
                    showMessageBox('Nenhuma câmera ativa para capturar.');
                }
            });
        });

        // Event listeners for "Adicionar Arquivo" buttons
        document.querySelectorAll('.add-file-button').forEach(button => {
            button.addEventListener('click', (event) => {
                const targetInputId = event.target.dataset.targetInput;
                const fileInput = document.getElementById(targetInputId);
                if (fileInput) {
                    stopCameraStream(); // Stop camera if active
                    fileInput.click(); // Programmatically click the hidden file input
                }
            });
        });

        // Event listeners for hidden file inputs
        document.querySelectorAll('input[type="file"]').forEach(input => {
            input.addEventListener('change', (event) => {
                const file = event.target.files[0];
                if (file) {
                    const targetImgId = event.target.dataset.targetImg;
                    const targetCommentId = event.target.dataset.targetComment;
                    const imgElement = document.getElementById(targetImgId);
                    const commentElement = document.getElementById(targetCommentId);

                    const reader = new FileReader();
                    reader.onload = (e) => {
                        if (imgElement) {
                            imgElement.src = e.target.result;
                            imgElement.style.display = 'block'; // Show the image
                        }
                        if (commentElement) {
                            commentElement.style.display = 'block'; // Show the comment textarea
                        }
                        showMessageBox('Arquivo adicionado!');
                    };
                    reader.readAsDataURL(file);
                } else {
                    showMessageBox('Nenhum arquivo selecionado.');
                }
            });
        });

        // Event listener for Generate Report button
        document.getElementById('generateReportButton').addEventListener('click', () => {
            stopCameraStream(); // Ensure camera is off before printing
            window.print(); // Triggers the browser's print dialog
        });

        // Set current date on load
        document.addEventListener('DOMContentLoaded', () => {
            const today = new Date();
            const yyyy = today.getFullYear();
            const mm = String(today.getMonth() + 1).padStart(2, '0'); // Months are 0-indexed
            const dd = String(today.getDate()).padStart(2, '0');
            document.getElementById('reportDate').value = `${yyyy}-${mm}-${dd}`;
        });
    </script>
</body>
</html>
