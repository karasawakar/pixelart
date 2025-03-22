<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        .pixel {
            width: 10px; /* 半分のサイズに変更 */
            height: 10px; /* 半分のサイズに変更 */
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
        <label for="colors">Choose color:</label>
        <input type="color" id="color" value="#ff0000">
    </div>
    <div id="buttons">
        <button id="clear">Clear</button>
    </div>
    <div id="pixel-art"></div>

    <script>
        const pixelArt = document.getElementById('pixel-art');
        const colorPicker = document.getElementById('color');
        const clearButton = document.getElementById('clear');

        // ピクセルアートを生成する関数
        function createPixelArt(rows, cols) {
            pixelArt.innerHTML = '';  // Clear existing pixel art
            for (let i = 0; i < rows; i++) {
                const rowDiv = document.createElement('div');
                rowDiv.classList.add('row');
                for (let j = 0; j < cols; j++) {
                    const pixelDiv = document.createElement('div');
                    pixelDiv.classList.add('pixel');
                    pixelDiv.addEventListener('click', () => {
                        pixelDiv.style.backgroundColor = colorPicker.value;
                    });
                    rowDiv.appendChild(pixelDiv);
                }
                pixelArt.appendChild(rowDiv);
            }
        }

        // クリアボタンのクリックイベント
        clearButton.addEventListener('click', () => createPixelArt(24, 24));

        // 24×24のピクセルアートを生成
        createPixelArt(24, 24);
    </script>
</body>
</html>
