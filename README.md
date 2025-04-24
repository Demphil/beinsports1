<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Foottv Live</title>
  <script src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script>
  <style>
    body, html {
      margin: 0;
      padding: 0;
      background: #000;
      height: 100%;
      overflow: hidden;
    }
    #video {
      width: 100%;
      height: 100vh;
      object-fit: cover;
    }
    .overlay {
      position: absolute;
      top: 10px;
      left: 10px;
      z-index: 1000;
      display: flex;
      gap: 10px;
      align-items: center;
    }
    .overlay img {
      width: 40px;
      height: 40px;
    }
    .overlay a, .overlay button {
      color: white;
      background: rgba(0,0,0,0.6);
      border: none;
      padding: 5px 10px;
      border-radius: 5px;
      text-decoration: none;
      font-size: 14px;
      cursor: pointer;
    }
    .overlay a:hover, .overlay button:hover {
      background: rgba(255,255,255,0.1);
    }
  </style>
</head>
<body>
  <div class="overlay">
    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/19/Football_Logo.svg/1024px-Football_Logo.svg.png" alt="Logo">
    <a href="https://foottv.info" target="_blank">زورونا: foottv.info</a>
    <button onclick="shareTeam()">مشاركة الفريق</button>
    <button onclick="refreshVideo()">تحديث الفيديو</button>
  </div>

  <video id="video" controls autoplay></video>

  <div >
  <video id="video" controls autoplay width="100%"></video>
  <div style="position: absolute; bottom: 40px; left: 25%; width: 19%; height: 33px; background: GREEN; opacity: 0.1;"></div>
</div>
<script src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script>
<script>
  var video = document.getElementById('video');
  var container = document.getElementById('videoContainer');
  var overlay = document.getElementById('videoOverlay');

  if (Hls.isSupported()) {
    var hls = new Hls();
    hls.loadSource('https://stream.sainaertebat.com/hls2/bein1.m3u8');
    hls.attachMedia(video);
    hls.on(Hls.Events.MANIFEST_PARSED, function () {
      video.play();
    });
  } else if (video.canPlayType('application/vnd.apple.mpegurl')) {
    video.src = 'https://stream.sainaertebat.com/hls2/bein1.m3u8';
    video.addEventListener('loadedmetadata', function () {
      video.play();
    });
  }

  // التعامل مع fullscreen
  function appendOverlayToFullscreen() {
    setTimeout(() => {
      const fullscreenElement = document.fullscreenElement || document.webkitFullscreenElement;
      if (fullscreenElement && fullscreenElement.tagName === 'VIDEO') {
        fullscreenElement.parentElement.appendChild(overlay);
        overlay.style.position = 'fixed';
      } else if (fullscreenElement && fullscreenElement.id === 'videoContainer') {
        container.appendChild(overlay);
        overlay.style.position = 'absolute';
      }
    }, 300);
  }

  document.addEventListener('fullscreenchange', appendOverlayToFullscreen);
  document.addEventListener('webkitfullscreenchange', appendOverlayToFullscreen);
</script>

</body>
</html>
