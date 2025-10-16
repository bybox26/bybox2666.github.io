<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RGB颜色混合实验室</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            transition: background 0.5s ease;
        }
        
        .container {
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            padding: 30px;
            width: 100%;
            max-width: 500px;
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
            border-radius: 5px;
            outline: none;
            -webkit-appearance: none;
        }
        
        .slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            cursor: pointer;
        }
        
        #red-slider {
            background: linear-gradient(to right, #000, red);
        }
        
        #red-slider::-webkit-slider-thumb {
            background: red;
        }
        
        #green-slider {
            background: linear-gradient(to right, #000, green);
        }
        
        #green-slider::-webkit-slider-thumb {
            background: green;
        }
        
        #blue-slider {
            background: linear-gradient(to right, #000, blue);
        }
        
        #blue-slider::-webkit-slider-thumb {
            background: blue;
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
            background-color: #f9f9f9;
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
        
        .value-display {
            font-family: monospace;
            font-size: 14px;
            color: #666;
            text-align: right;
        }
        
        .footer {
            margin-top: 30px;
            text-align: center;
            color: #777;
            font-size: 14px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>RGB颜色混合实验室</h1>
        
        <div class="color-display" id="color-display"></div>
        
        <div class="slider-container">
            <div class="slider-label">
                <span>红色 (R)</span>
                <span class="value-display" id="red-value">0</span>
            </div>
            <input type="range" min="0" max="255" value="0" class="slider" id="red-slider">
        </div>
        
        <div class="slider-container">
            <div class="slider-label">
                <span>绿色 (G)</span>
                <span class="value-display" id="green-value">0</span>
            </div>
            <input type="range" min="0" max="255" value="0" class="slider" id="green-slider">
        </div>
        
        <div class="slider-container">
            <div class="slider-label">
                <span>蓝色 (B)</span>
                <span class="value-display" id="blue-value">0</span>
            </div>
            <input type="range" min="0" max="255" value="0" class="slider" id="blue-slider">
        </div>
        
        <div class="hex-container">
            <input type="text" id="hex-value" readonly>
            <button id="copy-btn">复制</button>
        </div>
    </div>
    
    <div class="footer">
        RGB颜色混合实验室 &copy; 2025
    </div>

    <script>
        // 获取DOM元素
        const colorDisplay = document.getElementById('color-display');
        const redSlider = document.getElementById('red-slider');
        const greenSlider = document.getElementById('green-slider');
        const blueSlider = document.getElementById('blue-slider');
        const redValue = document.getElementById('red-value');
        const greenValue = document.getElementById('green-value');
        const blueValue = document.getElementById('blue-value');
        const hexValue = document.getElementById('hex-value');
        const copyBtn = document.getElementById('copy-btn');
        
        // 更新颜色显示
        function updateColor() {
            const r = parseInt(redSlider.value);
            const g = parseInt(greenSlider.value);
            const b = parseInt(blueSlider.value);
            
            // 更新RGB值显示
            redValue.textContent = r;
            greenValue.textContent = g;
            blueValue.textContent = b;
            
            // 生成HEX颜色码
            const hex = rgbToHex(r, g, b);
            
            // 更新显示
            colorDisplay.style.backgroundColor = `rgb(${r}, ${g}, ${b})`;
            hexValue.value = hex;
            
            // 更新背景渐变
            updateBackground(r, g, b);
        }
        
        // RGB转HEX
        function rgbToHex(r, g, b) {
            return "#" + componentToHex(r) + componentToHex(g) + componentToHex(b);
        }
        
        function componentToHex(c) {
            const hex = c.toString(16);
            return hex.length == 1 ? "0" + hex : hex;
        }
        
        // 更新背景渐变
        function updateBackground(r, g, b) {
            // 计算互补色
            const compR = 255 - r;
            const compG = 255 - g;
            const compB = 255 - b;
            
            // 设置渐变背景
            document.body.style.background = 
                `linear-gradient(135deg, rgb(${r}, ${g}, ${b}) 0%, rgb(${compR}, ${compG}, ${compB}) 100%)`;
        }
        
        // 复制HEX值
        copyBtn.addEventListener('click', () => {
            hexValue.select();
            document.execCommand('copy');
            
            // 显示复制成功反馈
            const originalText = copyBtn.textContent;
            copyBtn.textContent = '已复制!';
            setTimeout(() => {
                copyBtn.textContent = originalText;
            }, 2000);
        });
        
        // 添加事件监听器
        redSlider.addEventListener('input', updateColor);
        greenSlider.addEventListener('input', updateColor);
        blueSlider.addEventListener('input', updateColor);
        
        // 初始化
        updateColor();
    </script>
</body>
</html>
