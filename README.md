# Silent-came
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>無音カメラ</title>
  <style>
    body { text-align: center; font-family: sans-serif; margin: 0; padding: 1em; background: #f5f5f5; }
    video { max-width: 100%; border-radius: 8px; }
    button { padding: 2em 4em; font-size: 2em; margin-top: 1.5em; border: none; border-radius: 12px; background-color: #007BFF; color: white; cursor: pointer; }
    button:hover { background-color: #0056b3; }
    #downloadLink { display: block; margin-top: 1.5em; font-size: 1.2em; }
  </style>
</head>
<body>
  <h1>📷 無音カメラ</h1>
  <video id="video" autoplay playsinline></video><br>
  <button id="snap">📸 撮影</button>
  <canvas id="canvas" style="display:none;"></canvas>
  <a id="downloadLink" style="display:none;">画像を保存</a>

  <script>
    const video = document.getElementById('video');
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');
    const link = document.getElementById('downloadLink');

    navigator.mediaDevices.getUserMedia({ video: true }).then(stream => {
      video.srcObject = stream;
    }).catch(err => {
      alert('カメラへのアクセスに失敗しました: ' + err);
    });

    document.getElementById('snap').onclick = () => {
      canvas.width = video.videoWidth;
      canvas.height = video.videoHeight;
      ctx.drawImage(video, 0, 0);
      canvas.toBlob(blob => {
        const url = URL.createObjectURL(blob);
        link.href = url;
        link.download = 'photo.jpg';
        link.textContent = '画像を保存';
        link.style.display = 'inline';
      }, 'image/jpeg');
    };
  </script>
</body>
</html>
