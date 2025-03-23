<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        .pixel {
            width: 20px;
            height: 20px;
            float: left;
            border: 1px solid #ddd;
        }
        .row {
            clear: both;
        }
        #color-picker {
            margin-bottom: 10px;
        }
        #buttons {
            margin-top: 10px;
        }
    </style>
    <title>Pixel Art</title>
</head>
<body>
    <div id="color-picker">
        <p style="color: red; font-size: 1.2em;">Save as JPGはPCで実施してください。スマホでは未検証です。</p>
        <label for="colors">Choose color:</label>
        <input type="color" id="color" value="#000000">
    </div>
    <div id="buttons">
        <button id="clear">Clear</button>
        <button id="switch24">24×24</button>
        <button id="switch36">36×36</button>
        <button id="zoom-in">Zoom in</button> <!-- 拡大ボタン -->
        <button id="zoom-out">Zoom out</button> <!-- 縮小ボタン -->
        <button id="save">Save as JPG</button>
    </div>
    <div id="pixel-art"></div>
    <canvas id="canvas" style="display:none;"></canvas>

    <script>
        const pixelArt = document.getElementById('pixel-art');
        const colorPicker = document.getElementById('color');
        const clearButton = document.getElementById('clear');
        const switch24Button = document.getElementById('switch24');
        const switch36Button = document.getElementById('switch36');
        const saveButton = document.getElementById('save');
        const zoomInButton = document.getElementById('zoom-in');
        const zoomOutButton = document.getElementById('zoom-out');
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        let isDragging = false; // ドラッグ状態を追跡するフラグ

        // ピクセルアートを生成する関数
        function createPixelArt(rows, cols) {
            pixelArt.innerHTML = ''; // Clear existing pixel art
            for (let i = 0; i < rows; i++) {
                const rowDiv = document.createElement('div');
                rowDiv.classList.add('row');
                for (let j = 0; j < cols; j++) {
                    const pixelDiv = document.createElement('div');
                    pixelDiv.classList.add('pixel');
                    pixelDiv.addEventListener('mousedown', () => {
                        isDragging = true;
                        pixelDiv.style.backgroundColor = colorPicker.value;
                    });
                    pixelDiv.addEventListener('mousemove', () => {
                        if (isDragging) {
                            pixelDiv.style.backgroundColor = colorPicker.value;
                        }
                    });
                    pixelDiv.addEventListener('mouseup', () => {
                        isDragging = false;
                    });
                    rowDiv.appendChild(pixelDiv);
                }
                pixelArt.appendChild(rowDiv);
            }
        }

        // グローバルなイベントリスナーでドラッグ終了を処理
        document.addEventListener('mouseup', () => {
            isDragging = false;
        });

        // ピクセルサイズを拡大する関数
        zoomInButton.addEventListener('click', () => {
            const pixels = document.querySelectorAll('.pixel');
            pixels.forEach(pixel => {
                const currentSize = parseInt(window.getComputedStyle(pixel).width);
                const newSize = currentSize + 5; // Increase size by 5px
                pixel.style.width = `${newSize}px`;
                pixel.style.height = `${newSize}px`;
            });
        });

        // ピクセルサイズを縮小する関数
        zoomOutButton.addEventListener('click', () => {
            const pixels = document.querySelectorAll('.pixel');
            pixels.forEach(pixel => {
                const currentSize = parseInt(window.getComputedStyle(pixel).width);
                const newSize = Math.max(currentSize - 5, 5); // Decrease size by 5px, minimum 5px
                pixel.style.width = `${newSize}px`;
                pixel.style.height = `${newSize}px`;
            });
        });

        // キャンバスにピクセルアートを描画する関数
        function drawToCanvas() {
            const pixels = document.querySelectorAll('.pixel');
            const rows = document.querySelectorAll('.row').length;
            const cols = pixels.length / rows;

            canvas.width = cols * 20;
            canvas.height = rows * 20;

            pixels.forEach((pixel, index) => {
                const x = (index % cols) * 20;
                const y = Math.floor(index / cols) * 20;
                ctx.fillStyle = pixel.style.backgroundColor || '#ffffff';
                ctx.fillRect(x, y, 20, 20);
            });
        }

        // 画像を保存する関数
        function saveAsJPG() {
            const filename = prompt('ファイル名を入力してください（拡張子を含めない）：');
            if (filename) {
                drawToCanvas();
                const link = document.createElement('a');
                link.href = canvas.toDataURL('image/jpeg');
                link.download = `${filename}.jpg`;
                link.click();
            }
        }

        // ボタンのクリックイベント
        saveButton.addEventListener('click', saveAsJPG);
        clearButton.addEventListener('click', () => createPixelArt(24, 24));
        switch24Button.addEventListener('click', () => createPixelArt(24, 24));
        switch36Button.addEventListener('click', () => createPixelArt(36, 36));

        // 初期設定（24×24マス）
        createPixelArt(24, 24);
    </script>
</body>
</html>
