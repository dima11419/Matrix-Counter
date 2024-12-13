<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Countdown Timer</title>
    <!-- SF Pro Display font -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        @supports (font: -apple-system-body) {
            body { font: -apple-system-body; }
        }

        body {
            margin: 0;
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: #000000;
            color: #ffffff;
            font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'SF Pro Text', system-ui, sans-serif;
            -webkit-font-smoothing: antialiased;
            overflow: hidden;
        }

        .container {
            text-align: center;
            padding: 2rem;
            max-width: 1000px;
            width: 100%;
        }

        h1 {
            font-size: 56px;
            font-weight: 700;
            margin-bottom: 1.5rem;
            background: linear-gradient(to right, #ffffff, #a8a8a8);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            letter-spacing: -0.5px;
        }

        .countdown {
            display: flex;
            justify-content: center;
            gap: 2rem;
            margin: 3rem auto;
            flex-wrap: wrap;
        }

        .time-block {
            display: flex;
            flex-direction: column;
            align-items: center;
            min-width: 120px;
        }

        .number {
            font-size: 64px;
            font-weight: 600;
            line-height: 1.1;
            background: linear-gradient(180deg, #ffffff 0%, #e0e0e0 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .label {
            font-size: 14px;
            font-weight: 500;
            color: #86868b;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-top: 0.5rem;
        }

        .hero-text {
            font-size: 21px;
            line-height: 1.5;
            color: #86868b;
            max-width: 600px;
            margin: 2rem auto;
            font-weight: 400;
        }

        .button-container {
            display: flex;
            justify-content: center;
            gap: 2rem;
            margin-top: 2rem;
        }

        .pill-group {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 0.5rem;
        }

        .pill-button {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            padding: 20px 40px;
            border-radius: 30px;
            color: #fff;
            font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', system-ui, sans-serif;
            font-size: 24px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
            backdrop-filter: blur(10px);
            min-width: 180px;
        }

        .pill-button:hover {
            background: rgba(255, 255, 255, 0.15);
            transform: scale(1.02);
        }

        .pill-button.red-active {
            background: rgba(255, 59, 48, 0.2);
            color: #ff3b30;
        }

        .pill-button.blue-active {
            background: rgba(0, 122, 255, 0.2);
            color: #007AFF;
        }

        .counter-group {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-top: 1rem;
            color: #86868b;
        }

        .counter-group i {
            font-size: 24px;
        }

        .pill-count {
            font-size: 28px;
            font-weight: 500;
        }

        @keyframes pop {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }

        .gradient-bg {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at center, #1a1a1a 0%, #000000 70%);
            opacity: 0.8;
            z-index: -1;
        }

    </style>
</head>
<body>
    <div class="gradient-bg"></div>
    <div class="container">
        <h1>The Future Awaits</h1>
        <p class="hero-text">Experience every moment as it unfolds.</p>
        <div class="countdown">
            <div class="time-block">
                <div id="months" class="number">00</div>
                <div class="label">Months</div>
            </div>
            <div class="time-block">
                <div id="days" class="number">00</div>
                <div class="label">Days</div>
            </div>
            <div class="time-block">
                <div id="hours" class="number">00</div>
                <div class="label">Hours</div>
            </div>
            <div class="time-block">
                <div id="minutes" class="number">00</div>
                <div class="label">Minutes</div>
            </div>
            <div class="time-block">
                <div id="seconds" class="number">00</div>
                <div class="label">Seconds</div>
            </div>
        </div>
        <div class="button-container">
            <div class="pill-group">
                <button id="bluePill" class="pill-button">
                    Blue Pill
                </button>
                <div class="counter-group">
                    <span class="pill-count">0</span>
                    <i class="fas fa-thumbs-up"></i>
                </div>
            </div>
            <div class="pill-group">
                <button id="redPill" class="pill-button">
                    Red Pill
                </button>
                <div class="counter-group">
                    <span class="pill-count">0</span>
                    <i class="fas fa-thumbs-up"></i>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Set the end date to 1 year from now
        let endDate = new Date();
        endDate.setFullYear(endDate.getFullYear() + 1);

        let redCount = 0;
        let blueCount = 0;

        function updateCountdown() {
            const now = new Date();
            let diff = endDate - now;

            // Adjust the end date based on pill clicks
            diff += (blueCount - redCount) * 24 * 60 * 60 * 1000; // Convert days to milliseconds

            if (diff <= 0) {
                document.querySelectorAll('.number').forEach(el => el.textContent = '00');
                return;
            }

            const months = Math.floor(diff / (1000 * 60 * 60 * 24 * 30.44));
            const days = Math.floor((diff % (1000 * 60 * 60 * 24 * 30.44)) / (1000 * 60 * 60 * 24));
            const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
            const seconds = Math.floor((diff % (1000 * 60)) / 1000);

            document.getElementById('months').textContent = String(months).padStart(2, '0');
            document.getElementById('days').textContent = String(days).padStart(2, '0');
            document.getElementById('hours').textContent = String(hours).padStart(2, '0');
            document.getElementById('minutes').textContent = String(minutes).padStart(2, '0');
            document.getElementById('seconds').textContent = String(seconds).padStart(2, '0');
        }

        setInterval(updateCountdown, 1000);
        updateCountdown();

        // Button functionality
        function handlePillClick(button, counter, isRed) {
            const count = isRed ? ++redCount : ++blueCount;
            counter.textContent = count;

            button.classList.add(isRed ? 'red-active' : 'blue-active');

            setTimeout(() => {
                button.classList.remove(isRed ? 'red-active' : 'blue-active');
            }, 200);
        }

        const redPill = document.getElementById('redPill');
        const bluePill = document.getElementById('bluePill');
        const redCounter = redPill.parentElement.querySelector('.pill-count');
        const blueCounter = bluePill.parentElement.querySelector('.pill-count');

        redPill.addEventListener('click', () => handlePillClick(redPill, redCounter, true));
        bluePill.addEventListener('click', () => handlePillClick(bluePill, blueCounter, false));
    </script>
</body>
</html>

Version 3 of 3
