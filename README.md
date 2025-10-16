<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RGB颜色混合实验室</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            margin: 0;
            padding: 20px;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            transition: background 0.5s ease;
        }
        
        .container {
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            padding: 30px;
            width: 90%;
            max-width: 600px;
            margin-top: 20px;
        }
        
        h1 {
            color: #333;
            text-align: center;
            margin-bottom: 30px;
        }
        
        .color-display {
            width: 100%;
            height: 150px;
            border-radius: 10px;
            margin-bottom: 30px;
            box-shadow: inset 0 0 10px rgba(0, 0, 0, 0.1);
            transition: background-color 0.3s ease;
        }
        
        .slider-container {
            margin-bottom: 20px;
        }
        
        .slider-label {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
        }
        
        .slider {
            width: 100%;
            height: 10px;
            -webkit-appearance: none;
            appearance: none;
            background: #e0e0e0;
            outline: none;
            border-radius: 5px;
        }
        
        .slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            cursor: pointer;
        }
        
        #red-slider::-webkit-slider-thumb {
            background: #ff0000;
        }
        
        #green-slider::-webkit-slider-thumb {
            background: #00ff00;
        }
        
        #blue-slider::-webkit-slider-thumb {
            background: #0000ff;
        }
        
        .hex-container {
            display: flex;
            align-items: center;
            margin-top: 30px;
        }
        
        #hex-value {
            flex-grow: 1;
            padding: 10px 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
            text-align: center;
            font-family: monospace;
        }
        
        #copy-btn {
            margin-left: 10px;
            padding: 10px 15px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }
        
        #copy-btn:hover {
            background-color: #45a049;
        }
        
        .copied-message {
            color: #4CAF50;
            font-size: 14px;
            margin-left: 10px;
            opacity: 0;
            transition: opacity 0.3s;
        }
        
        .visible {
            opacity: 1;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>RGB颜色混合实验室</h1>
        
        <div class="color-display" id="color-display"></div>
        
        <div class="slider-container">
            <div class="slider-label">
                <span>红色 (R): <span id="red-value">0</span></span>
                <span>0-255</span>
            </div>
            <input type="range" min="0" max="255" value="0" class="slider" id="red-slider">
        </div>
        
        <div class="slider-container">
            <div class="slider-label">
                <span>绿色 (G): <span id="green-value">0</span></span>
                <span>0-255</span>
            </div>
            <input type="range" min="0" max="255" value="0" class="slider" id="green-slider">
        </div>
        
        <div class="slider-container">
            <div class="slider-label">
                <span>蓝色 (B): <span id="blue-value">0</span></span>
                <span>0-255</span>
            </div>
            <input type="range" min="0" max="255" value="0" class="slider" id="blue-slider">
        </div>
        
        <div class="hex-container">
            <input type="text" id="hex-value" readonly>
            <button id="copy-btn">复制</button>
            <span class="copied-message" id="copied-message">已复制!</span>
        </div>
    </div>

    <script>
        // 获取DOM元素
        const redSlider = document.getElementById('red-slider');
        const greenSlider = document.getElementById('green-slider');
        const blueSlider = document.getElementById('blue-slider');
        
        const redValue = document.getElementById('red-value');
        const greenValue = document.getElementById('green-value');
        const blueValue = document.getElementById('blue-value');
        
        const colorDisplay = document.getElementById('color-display');
        const hexValue = document.getElementById('hex-value');
        const copyBtn = document.getElementById('copy-btn');
        const copiedMessage = document.getElementById('copied-message');
        
        // 更新颜色显示
        function updateColor() {
            const r = redSlider.value;
            const g = greenSlider.value;
            const b = blueSlider.value;
            
            // 更新数值显示
            redValue.textContent = r;
            greenValue.textContent = g;
            blueValue.textContent = b;
            
            // 转换为16进制
            const hex = rgbToHex(r, g, b);
            
            // 更新颜色显示
            colorDisplay.style.backgroundColor = `rgb(${r}, ${g}, ${b})`;
            hexValue.value = hex;
            
            // 更新背景渐变
            document.body.style.background = `linear-gradient(135deg, rgba(${r}, ${g}, ${b}, 0.2) 0%, rgba(${r}, ${g}, ${b}, 0.4) 100%)`;
        }
        
        // RGB转HEX
        function rgbToHex(r, g, b) {
            return "#" + componentToHex(r) + componentToHex(g) + componentToHex(b);
        }
        
        function componentToHex(c) {
            const hex = parseInt(c).toString(16);
            return hex.length == 1 ? "0" + hex : hex;
        }
        
        // 复制HEX值
        copyBtn.addEventListener('click', () => {
            hexValue.select();
            document.execCommand('copy');
            
            // 显示复制成功消息
            copiedMessage.classList.add('visible');
            setTimeout(() => {
                copiedMessage.classList.remove('visible');
            }, 2000);
        });
        
        // 添加滑块事件监听
        redSlider.addEventListener('input', updateColor);
        greenSlider.addEventListener('input', updateColor);
        blueSlider.addEventListener('input', updateColor);
        
        // 初始化
        updateColor();
    </script>
</body>
</html>
