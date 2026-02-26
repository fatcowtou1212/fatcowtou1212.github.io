<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>水果識別系統</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', 'Microsoft JhengHei', sans-serif;
        }
        
        body {
            background-color: #f5f9fc;
            color: #333;
            line-height: 1.6;
            padding: 20px;
            max-width: 1200px;
            margin: 0 auto;
        }
        
        header {
            text-align: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 2px solid #eaeaea;
        }
        
        h1 {
            color: #2c8c4a;
            margin-bottom: 10px;
            font-size: 2.5rem;
        }
        
        .subtitle {
            color: #666;
            font-size: 1.1rem;
        }
        
        .container {
            display: flex;
            flex-wrap: wrap;
            gap: 30px;
            margin-bottom: 40px;
        }
        
        .section {
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
            padding: 25px;
            flex: 1;
            min-width: 300px;
        }
        
        .section-title {
            color: #2c8c4a;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid #f0f7f2;
            font-size: 1.5rem;
        }
        
        .camera-container {
            position: relative;
            width: 100%;
            background-color: #222;
            border-radius: 10px;
            overflow: hidden;
            margin-bottom: 20px;
        }
        
        #videoElement {
            width: 100%;
            height: auto;
            display: block;
        }
        
        .camera-overlay {
            position: absolute;
            top: 10px;
            left: 10px;
            color: white;
            background-color: rgba(0, 0, 0, 0.5);
            padding: 5px 10px;
            border-radius: 5px;
            font-size: 0.9rem;
        }
        
        .controls {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            margin-top: 20px;
        }
        
        button {
            padding: 12px 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            flex: 1;
            min-width: 140px;
        }
        
        .btn-primary {
            background-color: #2c8c4a;
            color: white;
        }
        
        .btn-primary:hover {
            background-color: #237a3d;
            transform: translateY(-2px);
        }
        
        .btn-secondary {
            background-color: #4a6fa5;
            color: white;
        }
        
        .btn-secondary:hover {
            background-color: #3d5d8c;
            transform: translateY(-2px);
        }
        
        .btn-danger {
            background-color: #e74c3c;
            color: white;
        }
        
        .btn-danger:hover {
            background-color: #c0392b;
            transform: translateY(-2px);
        }
        
        .btn-warning {
            background-color: #f39c12;
            color: white;
        }
        
        .btn-warning:hover {
            background-color: #d68910;
            transform: translateY(-2px);
        }
        
        .btn:disabled {
            background-color: #cccccc;
            cursor: not-allowed;
            transform: none;
        }
        
        .recording-indicator {
            display: none;
            align-items: center;
            color: #e74c3c;
            font-weight: bold;
            margin-top: 15px;
        }
        
        .recording-dot {
            width: 12px;
            height: 12px;
            background-color: #e74c3c;
            border-radius: 50%;
            margin-right: 8px;
            animation: pulse 1.5s infinite;
        }
        
        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }
        
        .upload-area {
            border: 3px dashed #ccc;
            border-radius: 10px;
            padding: 40px 20px;
            text-align: center;
            margin-bottom: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .upload-area:hover, .upload-area.dragover {
            border-color: #2c8c4a;
            background-color: #f9fffb;
        }
        
        .upload-icon {
            font-size: 3rem;
            color: #2c8c4a;
            margin-bottom: 15px;
        }
        
        .result-container {
            background-color: #f9fffb;
            border-radius: 10px;
            padding: 20px;
            margin-top: 25px;
            border-left: 5px solid #2c8c4a;
            display: none;
        }
        
        .result-title {
            font-weight: bold;
            margin-bottom: 10px;
            color: #2c8c4a;
        }
        
        .fruit-name {
            font-size: 2.2rem;
            color: #e67e22;
            font-weight: bold;
            text-align: center;
            margin: 15px 0;
        }
        
        .fruit-image {
            max-width: 200px;
            max-height: 200px;
            display: block;
            margin: 0 auto 15px;
            border-radius: 10px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
        }
        
        .confidence {
            text-align: center;
            color: #666;
            font-size: 0.9rem;
        }
        
        .fruit-list {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            margin-top: 15px;
            justify-content: center;
        }
        
        .fruit-item {
            background-color: #f0f7f2;
            border-radius: 10px;
            padding: 10px 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .fruit-emoji {
            font-size: 1.5rem;
        }
        
        .file-list {
            margin-top: 20px;
            max-height: 200px;
            overflow-y: auto;
            border-top: 1px solid #eee;
            padding-top: 15px;
        }
        
        .file-item {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            border-bottom: 1px dashed #eee;
        }
        
        footer {
            text-align: center;
            margin-top: 40px;
            padding-top: 20px;
            border-top: 1px solid #eaeaea;
            color: #777;
            font-size: 0.9rem;
        }
        
        @media (max-width: 768px) {
            .container {
                flex-direction: column;
            }
            
            button {
                min-width: 100%;
            }
        }
    </style>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>
    <header>
        <h1><i class="fas fa-apple-alt"></i> 水果識別系統</h1>
        <p class="subtitle">使用您的攝像頭拍攝照片或錄影，上傳水果圖片進行識別</p>
    </header>
    
    <div class="container">
        <div class="section">
            <h2 class="section-title"><i class="fas fa-camera"></i> 攝像頭控制</h2>
            
            <div class="camera-container">
                <div class="camera-overlay">
                    <i class="fas fa-video"></i> 攝像頭預覽
                </div>
                <video id="videoElement" autoplay playsinline></video>
            </div>
            
            <div class="controls">
                <button id="startCamera" class="btn-primary">
                    <i class="fas fa-play"></i> 開啟攝像頭
                </button>
                <button id="stopCamera" class="btn-danger" disabled>
                    <i class="fas fa-stop"></i> 關閉攝像頭
                </button>
            </div>
            
            <div class="controls">
                <button id="capturePhoto" class="btn-secondary" disabled>
                    <i class="fas fa-camera"></i> 拍攝照片
                </button>
                <button id="startRecording" class="btn-warning" disabled>
                    <i class="fas fa-circle"></i> 開始錄影
                </button>
                <button id="stopRecording" class="btn-danger" disabled>
                    <i class="fas fa-square"></i> 停止錄影
                </button>
            </div>
            
            <div id="recordingIndicator" class="recording-indicator">
                <div class="recording-dot"></div>
                <span>正在錄影中...</span>
            </div>
            
            <div class="file-list" id="fileList">
                <p><strong>已儲存的檔案：</strong></p>
                <!-- 檔案列表將在這裡動態生成 -->
            </div>
        </div>
        
        <div class="section">
            <h2 class="section-title"><i class="fas fa-upload"></i> 水果識別</h2>
            
            <div class="upload-area" id="uploadArea">
                <div class="upload-icon">
                    <i class="fas fa-cloud-upload-alt"></i>
                </div>
                <p>拖放水果圖片到這裡，或點擊選擇檔案</p>
                <p><small>支援的格式: JPG, PNG, GIF</small></p>
                <input type="file" id="fileInput" accept="image/*" style="display:none;">
            </div>
            
            <div class="result-container" id="resultContainer">
                <div class="result-title">識別結果：</div>
                <img id="fruitImage" class="fruit-image" src="" alt="水果圖片">
                <div id="fruitName" class="fruit-name">蘋果</div>
                <div id="confidence" class="confidence">識別信心度：92%</div>
            </div>
            
            <div>
                <h3 class="section-title"><i class="fas fa-list"></i> 可識別的水果種類</h3>
                <div class="fruit-list">
                    <div class="fruit-item"><span class="fruit-emoji">🍎</span>蘋果</div>
                    <div class="fruit-item"><span class="fruit-emoji">🍌</span>香蕉</div>
                    <div class="fruit-item"><span class="fruit-emoji">🍊</span>橙</div>
                    <div class="fruit-item"><span class="fruit-emoji">🥭</span>芒果</div>
                    <div class="fruit-item"><span class="fruit-emoji">🍐</span>梨子</div>
                    <div class="fruit-item"><span class="fruit-emoji">🍉</span>西瓜</div>
                    <div class="fruit-item"><span class="fruit-emoji">🍓</span>草莓</div>
                    <div class="fruit-item"><span class="fruit-emoji">🍍</span>菠蘿</div>
                </div>
            </div>
        </div>
    </div>
    
    <footer>
        <p>水果識別系統 &copy; 2023 | 使用HTML5、MediaDevices API和Canvas技術</p>
        <p><small>注意：所有檔案都儲存在您的本地電腦中，不會上傳到任何伺服器</small></p>
    </footer>

    <script>
        // DOM元素
        const videoElement = document.getElementById('videoElement');
        const startCameraBtn = document.getElementById('startCamera');
        const stopCameraBtn = document.getElementById('stopCamera');
        const capturePhotoBtn = document.getElementById('capturePhoto');
        const startRecordingBtn = document.getElementById('startRecording');
        const stopRecordingBtn = document.getElementById('stopRecording');
        const recordingIndicator = document.getElementById('recordingIndicator');
        const uploadArea = document.getElementById('uploadArea');
        const fileInput = document.getElementById('fileInput');
        const resultContainer = document.getElementById('resultContainer');
        const fruitImage = document.getElementById('fruitImage');
        const fruitName = document.getElementById('fruitName');
        const confidence = document.getElementById('confidence');
        const fileList = document.getElementById('fileList');
        
        // 變數
        let mediaStream = null;
        let mediaRecorder = null;
        let recordedChunks = [];
        let savedFiles = JSON.parse(localStorage.getItem('fruitRecognizerFiles')) || [];
        
        // 初始化
        function init() {
            updateFileList();
            
            // 上傳區域點擊事件
            uploadArea.addEventListener('click', () => {
                fileInput.click();
            });
            
            // 檔案選擇事件
            fileInput.addEventListener('change', handleFileSelect);
            
            // 拖放事件
            uploadArea.addEventListener('dragover', (e) => {
                e.preventDefault();
                uploadArea.classList.add('dragover');
            });
            
            uploadArea.addEventListener('dragleave', () => {
                uploadArea.classList.remove('dragover');
            });
            
            uploadArea.addEventListener('drop', (e) => {
                e.preventDefault();
                uploadArea.classList.remove('dragover');
                
                if (e.dataTransfer.files.length) {
                    handleImageFile(e.dataTransfer.files[0]);
                }
            });
        }
        
        // 開啟攝像頭
        startCameraBtn.addEventListener('click', async () => {
            try {
                mediaStream = await navigator.mediaDevices.getUserMedia({
                    video: { width: 1280, height: 720 },
                    audio: false
                });
                
                videoElement.srcObject = mediaStream;
                
                // 啟用按鈕
                startCameraBtn.disabled = true;
                stopCameraBtn.disabled = false;
                capturePhotoBtn.disabled = false;
                startRecordingBtn.disabled = false;
                
                console.log('攝像頭已開啟');
            } catch (err) {
                console.error('無法開啟攝像頭:', err);
                alert('無法存取攝像頭。請確保您已授予相應的權限。');
            }
        });
        
        // 關閉攝像頭
        stopCameraBtn.addEventListener('click', () => {
            if (mediaStream) {
                // 停止所有軌道
                mediaStream.getTracks().forEach(track => track.stop());
                videoElement.srcObject = null;
                mediaStream = null;
                
                // 重置按鈕狀態
                startCameraBtn.disabled = false;
                stopCameraBtn.disabled = true;
                capturePhotoBtn.disabled = true;
                startRecordingBtn.disabled = true;
                stopRecordingBtn.disabled = true;
                
                // 停止錄影（如果正在錄影）
                if (mediaRecorder && mediaRecorder.state !== 'inactive') {
                    mediaRecorder.stop();
                    recordingIndicator.style.display = 'none';
                }
                
                console.log('攝像頭已關閉');
            }
        });
        
        // 拍攝照片
        capturePhotoBtn.addEventListener('click', () => {
            if (!mediaStream) return;
            
            const canvas = document.createElement('canvas');
            canvas.width = videoElement.videoWidth;
            canvas.height = videoElement.videoHeight;
            
            const context = canvas.getContext('2d');
            context.drawImage(videoElement, 0, 0, canvas.width, canvas.height);
            
            // 儲存照片
            canvas.toBlob(blob => {
                saveFile(blob, `fruit_photo_${Date.now()}.png`, 'image/png');
                // 識別水果
                recognizeFruitFromCanvas(canvas);
            }, 'image/png');
        });
        
        // 開始錄影
        startRecordingBtn.addEventListener('click', () => {
            if (!mediaStream) return;
            
            recordedChunks = [];
            
            try {
                mediaRecorder = new MediaRecorder(mediaStream, {
                    mimeType: 'video/webm;codecs=vp9'
                });
                
                mediaRecorder.ondataavailable = (event) => {
                    if (event.data.size > 0) {
                        recordedChunks.push(event.data);
                    }
                };
                
                mediaRecorder.onstop = () => {
                    const blob = new Blob(recordedChunks, { type: 'video/webm' });
                    saveFile(blob, `fruit_video_${Date.now()}.webm`, 'video/webm');
                };
                
                mediaRecorder.start();
                recordingIndicator.style.display = 'flex';
                startRecordingBtn.disabled = true;
                stopRecordingBtn.disabled = false;
                
                console.log('開始錄影');
            } catch (err) {
                console.error('無法開始錄影:', err);
                alert('您的瀏覽器不支援錄影功能，或編解碼器不兼容。');
            }
        });
        
        // 停止錄影
        stopRecordingBtn.addEventListener('click', () => {
            if (mediaRecorder && mediaRecorder.state !== 'inactive') {
                mediaRecorder.stop();
                recordingIndicator.style.display = 'none';
                startRecordingBtn.disabled = false;
                stopRecordingBtn.disabled = true;
                
                console.log('停止錄影');
            }
        });
        
        // 處理檔案選擇
        function handleFileSelect(event) {
            const file = event.target.files[0];
            if (file) {
                handleImageFile(file);
            }
        }
        
        // 處理圖片檔案
        function handleImageFile(file) {
            // 檢查是否為圖片
            if (!file.type.match('image.*')) {
                alert('請選擇圖片檔案！');
                return;
            }
            
            const reader = new FileReader();
            
            reader.onload = function(e) {
                // 顯示圖片
                fruitImage.src = e.target.result;
                resultContainer.style.display = 'block';
                
                // 識別水果
                recognizeFruit(file);
            };
            
            reader.readAsDataURL(file);
        }
        
        // 從Canvas識別水果
        function recognizeFruitFromCanvas(canvas) {
            // 將Canvas轉換為Blob
            canvas.toBlob(blob => {
                recognizeFruit(blob);
                // 顯示圖片
                fruitImage.src = canvas.toDataURL();
                resultContainer.style.display = 'block';
            }, 'image/png');
        }
        
        // 水果識別函數（模擬）
        function recognizeFruit(imageFile) {
            // 模擬識別過程
            const fruits = [
                { name: '蘋果', emoji: '🍎', confidence: 92 },
                { name: '香蕉', emoji: '🍌', confidence: 88 },
                { name: '橙', emoji: '🍊', confidence: 85 },
                { name: '芒果', emoji: '🥭', confidence: 79 },
                { name: '梨子', emoji: '🍐', confidence: 83 },
                { name: '西瓜', emoji: '🍉', confidence: 91 },
                { name: '草莓', emoji: '🍓', confidence: 87 },
                { name: '菠蘿', emoji: '🍍', confidence: 76 }
            ];
            
            // 隨機選擇一個水果（模擬識別結果）
            const randomFruit = fruits[Math.floor(Math.random() * fruits.length)];
            
            // 顯示結果
            fruitName.textContent = `${randomFruit.emoji} ${randomFruit.name}`;
            confidence.textContent = `識別信心度：${randomFruit.confidence}%`;
            
            console.log(`識別結果：${randomFruit.name} (${randomFruit.confidence}%)`);
        }
        
        // 儲存檔案到本地
        function saveFile(blob, filename, type) {
            // 創建下載連結
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = filename;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
            
            // 儲存檔案記錄
            const fileRecord = {
                name: filename,
                type: type,
                date: new Date().toLocaleString(),
                size: blob.size
            };
            
            savedFiles.unshift(fileRecord);
            
            // 只保留最近10個檔案記錄
            if (savedFiles.length > 10) {
                savedFiles = savedFiles.slice(0, 10);
            }
            
            // 儲存到localStorage
            localStorage.setItem('fruitRecognizerFiles', JSON.stringify(savedFiles));
            
            // 更新檔案列表
            updateFileList();
            
            console.log(`檔案已儲存：${filename}`);
        }
        
        // 更新檔案列表
        function updateFileList() {
            if (savedFiles.length === 0) {
                fileList.innerHTML = '<p><strong>已儲存的檔案：</strong></p><p>還沒有儲存任何檔案</p>';
                return;
            }
            
            let fileListHTML = '<p><strong>已儲存的檔案：</strong></p>';
            
            savedFiles.forEach(file => {
                const fileTypeIcon = file.type.includes('image') ? '🖼️' : '🎥';
                const size = (file.size / 1024).toFixed(1);
                
                fileListHTML += `
                    <div class="file-item">
                        <div>
                            ${fileTypeIcon} ${file.name}
                        </div>
                        <div style="color: #666; font-size: 0.9rem;">
                            ${file.date} (${size} KB)
                        </div>
                    </div>
                `;
            });
            
            fileList.innerHTML = fileListHTML;
        }
        
        // 頁面載入時初始化
        window.addEventListener('DOMContentLoaded', init);
        
        // 離開頁面前關閉攝像頭
        window.addEventListener('beforeunload', () => {
            if (mediaStream) {
                mediaStream.getTracks().forEach(track => track.stop());
            }
        });
    </script>
</body>
</html>
