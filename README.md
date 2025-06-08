<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Phone Animation App</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- Bootstrap CSS CDN -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            background: #f0f2f5;
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .phone-frame {
            width: 320px;
            height: 600px;
            background: #222;
            border-radius: 40px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.3);
            position: relative;
            overflow: hidden;
            border: 8px solid #444;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
        }
        .screen {
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #4e54c8 0%, #8f94fb 100%);
            position: absolute;
            top: 0;
            left: 0;
            z-index: 1;
            overflow: hidden;
            padding: 20px 0 0 0;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        /* Animated gradient waves */
        .waves {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            z-index: 0;
            pointer-events: none;
        }
        .waves svg {
            width: 100%;
            height: 100%;
            display: block;
            animation: waveMove 8s linear infinite;
        }
        @keyframes waveMove {
            0% { transform: translateX(0); }
            100% { transform: translateX(-50px); }
        }
        .bubble {
            min-width: 60px;
            max-width: 220px;
            margin: 10px 0;
            padding: 12px 18px;
            border-radius: 20px;
            color: #fff;
            font-size: 1rem;
            opacity: 0;
            transform: translateY(30px) scale(0.8);
            animation: bubblePop 0.6s cubic-bezier(.68,-0.55,.27,1.55) forwards;
            background: #6a82fb;
            align-self: flex-end;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
            position: relative;
            z-index: 1;
        }
        .bubble.left {
            background: #fff;
            color: #333;
            align-self: flex-start;
        }
        @keyframes bubblePop {
            0% {
                opacity: 0;
                transform: translateY(30px) scale(0.8);
            }
            60% {
                opacity: 1;
                transform: translateY(-10px) scale(1.1);
            }
            80% {
                transform: translateY(2px) scale(0.97);
            }
            100% {
                opacity: 1;
                transform: translateY(0) scale(1);
            }
        }
        .phone-notch {
            width: 120px;
            height: 18px;
            background: #222;
            border-radius: 0 0 20px 20px;
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            z-index: 2;
        }
        .app-bar {
            width: 100%;
            height: 48px;
            background: rgba(255,255,255,0.1);
            display: flex;
            align-items: center;
            justify-content: center;
            color: #fff;
            font-weight: 500;
            font-size: 1.1rem;
            z-index: 2;
            position: relative;
        }
        .input-group {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            width: 90%;
            z-index: 3;
        }
        .form-control {
            border-radius: 20px;
            border: none;
            padding-left: 18px;
        }
        .btn-send {
            border-radius: 50%;
            width: 40px;
            height: 40px;
            padding: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            background: #6a82fb;
            color: #fff;
            border: none;
            transition: transform 0.15s cubic-bezier(.68,-0.55,.27,1.55);
        }
        .btn-send:active {
            background: #4e54c8;
        }
        .btn-send.animated {
            animation: sendBounce 0.4s cubic-bezier(.68,-0.55,.27,1.55);
        }
        @keyframes sendBounce {
            0% { transform: scale(1);}
            30% { transform: scale(1.2);}
            60% { transform: scale(0.95);}
            100% { transform: scale(1);}
        }
    </style>
</head>
<body>
    <div class="phone-frame shadow">
        <div class="phone-notch"></div>
        <div class="app-bar">Chat App</div>
        <div class="screen" id="chatScreen">
            <div class="waves">
                <svg viewBox="0 0 320 120" preserveAspectRatio="none">
                    <defs>
                        <linearGradient id="waveGradient" x1="0" y1="0" x2="0" y2="1">
                            <stop offset="0%" stop-color="#8f94fb" stop-opacity="0.5"/>
                            <stop offset="100%" stop-color="#4e54c8" stop-opacity="0.2"/>
                        </linearGradient>
                    </defs>
                    <path d="M0,40 Q80,80 160,40 T320,40 V120 H0 Z" fill="url(#waveGradient)">
                        <animate attributeName="d" dur="6s" repeatCount="indefinite"
                            values="
                                M0,40 Q80,80 160,40 T320,40 V120 H0 Z;
                                M0,30 Q80,60 160,30 T320,40 V120 H0 Z;
                                M0,40 Q80,80 160,40 T320,40 V120 H0 Z
                            " />
                    </path>
                </svg>
            </div>
        </div>
        <form class="input-group" id="chatForm" autocomplete="off">
            <input type="text" class="form-control" id="chatInput" placeholder="Type a message..." required>
            <button class="btn btn-send ms-2" type="submit" id="sendBtn">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-send" viewBox="0 0 16 16">
                    <path d="M15.964 0.686a.5.5 0 0 1 .036.708l-15 15a.5.5 0 0 1-.708-.708l15-15a.5.5 0 0 1 .672 0z"/>
                    <path d="M6.5 10.5v3.793l7.146-7.147-3.793-.001a.5.5 0 0 1 0-1h5a.5.5 0 0 1 .5.5v5a.5.5 0 0 1-1 0v-3.793l-7.146 7.147a.5.5 0 0 1-.708-.708l7.147-7.146H6.5a.5.5 0 0 1 0-1h5a.5.5 0 0 1 .5.5v5a.5.5 0 0 1-1 0v-3.793l-7.146 7.147a.5.5 0 0 1-.708-.708l7.147-7.146z"/>
                </svg>
            </button>
        </form>
    </div>

    <!-- Bootstrap JS CDN -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        const chatScreen = document.getElementById('chatScreen');
        const chatForm = document.getElementById('chatForm');
        const chatInput = document.getElementById('chatInput');
        const sendBtn = document.getElementById('sendBtn');

        function addBubble(text, side = 'right') {
            const bubble = document.createElement('div');
            bubble.className = 'bubble' + (side === 'left' ? ' left' : '');
            bubble.textContent = text;
            chatScreen.appendChild(bubble);
            // Animate bubble in
            setTimeout(() => {
                bubble.style.animationDelay = '0s';
            }, 10);
            // Scroll to bottom
            chatScreen.scrollTop = chatScreen.scrollHeight;
        }

        chatForm.addEventListener('submit', function(e) {
            e.preventDefault();
            const msg = chatInput.value.trim();
            if (!msg) return;
            addBubble(msg, 'right');
            chatInput.value = '';
            // Animate send button
            sendBtn.classList.add('animated');
            setTimeout(() => sendBtn.classList.remove('animated'), 400);
            setTimeout(() => {
                // Simulate reply
                addBubble('Echo: ' + msg, 'left');
            }, 800);
        });

        // Initial animation
        window.onload = () => {
            setTimeout(() => addBubble('Welcome to the animated chat!', 'left'), 400);
        };

        // Animate background waves (optional JS for more effect)
        // You can add more JS-based SVG morphing or color shifting here if desired.
    </script>
</body>
</html>
