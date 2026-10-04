<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
    <title>Muza Alarm App</title>
    <style>
        :root {
            --bg: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
            --card: rgba(255, 255, 255, 0.08);
            --text: #ffffff;
            --input-bg: rgba(255, 255, 255, 0.12);
            --primary: linear-gradient(to right, #ff6b6b, #ee5a24);
        }

        body.light {
            --bg: linear-gradient(135deg, #f5f7fa, #c3cfe2);
            --card: rgba(255, 255, 255, 0.85);
            --text: #222;
            --input-bg: rgba(0, 0, 0, 0.06);
            --primary: linear-gradient(to right, #ff6b6b, #ee5a24);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', system-ui, sans-serif;
        }

        body {
            background: var(--bg);
            min-height: 100vh;
            color: var(--text);
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 16px;
            transition: all 0.4s ease;
        }

        .header {
            width: 100%;
            max-width: 440px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        h1 {
            font-size: 1.9rem;
            background: linear-gradient(to right, #ff6b6b, #feca57, #48dbfb);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .theme-btn {
            background: var(--card);
            border: none;
            width: 44px;
            height: 44px;
            border-radius: 50%;
            font-size: 1.3rem;
            cursor: pointer;
            color: var(--text);
        }

        .clock {
            font-size: 3.8rem;
            font-weight: 700;
            letter-spacing: 3px;
            margin: 12px 0 4px;
        }

        .date {
            font-size: 1.1rem;
            opacity: 0.75;
            margin-bottom: 22px;
        }

        .container {
            background: var(--card);
            backdrop-filter: blur(14px);
            border-radius: 22px;
            padding: 24px;
            width: 100%;
            max-width: 440px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.25);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .form-group {
            margin-bottom: 16px;
        }

        label {
            display: block;
            margin-bottom: 6px;
            font-size: 0.92rem;
            opacity: 0.9;
        }

        input[type="time"],
        input[type="text"] {
            width: 100%;
            padding: 13px 14px;
            border: none;
            border-radius: 12px;
            background: var(--input-bg);
            color: var(--text);
            font-size: 1.1rem;
            outline: none;
        }

        .days {
            display: flex;
            gap: 6px;
            flex-wrap: wrap;
            margin-top: 6px;
        }

        .day {
            width: 38px;
            height: 38px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.8rem;
            font-weight: 600;
            background: var(--input-bg);
            cursor: pointer;
            user-select: none;
            transition: 0.2s;
        }

        .day.active {
            background: #ff6b6b;
            color: white;
        }

        button.btn-primary {
            width: 100%;
            padding: 15px;
            border: none;
            border-radius: 14px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            background: var(--primary);
            color: white;
            margin-top: 8px;
            transition: 0.25s;
        }

        button.btn-primary:active {
            transform: scale(0.97);
        }

        .alarms-list {
            margin-top: 26px;
        }

        .alarm-item {
            background: var(--input-bg);
            border-radius: 16px;
            padding: 14px 16px;
            margin-bottom: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            animation: slideIn 0.35s ease;
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateY(15px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .alarm-time {
            font-size: 1.5rem;
            font-weight: 700;
        }

        .alarm-label {
            font-size: 0.88rem;
            opacity: 0.8;
            margin-top: 2px;
        }

        .alarm-days {
            font-size: 0.75rem;
            opacity: 0.7;
            margin-top: 4px;
        }

        .delete-btn {
            background: #ff4757;
            color: white;
            border: none;
            width: 36px;
            height: 36px;
            border-radius: 50%;
            font-size: 1.25rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .empty {
            text-align: center;
            opacity: 0.55;
            padding: 28px 10px;
            font-size: 0.95rem;
        }

        /* Ringing Screen */
        .ringing {
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.94);
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 999;
            animation: pulseBg 1.2s infinite;
        }

        @keyframes pulseBg {
            0%, 100% { background: rgba(180, 20, 20, 0.95); }
            50% { background: rgba(0, 0, 0, 0.96); }
        }

        .ringing h2 {
            font-size: 2.8rem;
            margin-bottom: 8px;
            animation: shake 0.45s infinite;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-12px); }
            75% { transform: translateX(12px); }
        }

        .ringing .big-time {
            font-size: 4.5rem;
            font-weight: 800;
            margin: 12px 0;
        }

        .ringing .label {
            font-size: 1.4rem;
            margin-bottom: 35px;
            opacity: 0.9;
        }

        .ring-btns {
            display: flex;
            gap: 14px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .ring-btns button {
            padding: 16px 28px;
            border: none;
            border-radius: 14px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            min-width: 140px;
        }

        .snooze-btn {
            background: #feca57;
            color: #222;
        }

        .stop-btn {
            background: #ff4757;
            color: white;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>Muza Alarm</h1>
        <button class="theme-btn" onclick="toggleTheme()">🌓</button>
    </div>

    <div class="clock" id="currentTime">00:00:00</div>
    <div class="date" id="currentDate"></div>

    <div class="container">
        <div class="form-group">
            <label>Time</label>
            <input type="time" id="alarmTime">
        </div>

        <div class="form-group">
            <label>Label</label>
            <input type="text" id="alarmLabel" placeholder="Utho bhai 😂">
        </div>

        <div class="form-group">
            <label>Repeat</label>
            <div class="days" id="daysContainer">
                <div class="day" data-day="0">S</div>
                <div class="day" data-day="1">M</div>
                <div class="day" data-day="2">T</div>
                <div class="day" data-day="3">W</div>
                <div class="day" data-day="4">T</div>
                <div class="day" data-day="5">F</div>
                <div class="day" data-day="6">S</div>
            </div>
        </div>

        <button class="btn-primary" onclick="setAlarm()">Set Alarm</button>

        <div class="alarms-list" id="alarmsList">
            <div class="empty">No alarms yet</div>
        </div>
    </div>

    <!-- Ringing Overlay -->
    <div class="ringing" id="ringingScreen">
        <h2>WAKE UP!</h2>
        <div class="big-time" id="ringingTime"></div>
        <div class="label" id="ringingLabel"></div>
        <div class="ring-btns">
            <button class="snooze-btn" onclick="snoozeAlarm()">Snooze 5 min</button>
            <button class="stop-btn" onclick="stopAlarm()">Stop</button>
        </div>
    </div>

    <script>
        let alarms = JSON.parse(localStorage.getItem('muzaAlarms')) || [];
        let selectedDays = [];
        let ringingId = null;
        let audioCtx = null;
        let oscillator = null;
        let soundInterval = null;

        // Theme
        function toggleTheme() {
            document.body.classList.toggle('light');
            localStorage.setItem('theme', document.body.classList.contains('light') ? 'light' : 'dark');
        }
        if (localStorage.getItem('theme') === 'light') {
            document.body.classList.add('light');
        }

        // Days selection
        document.querySelectorAll('.day').forEach(day => {
            day.addEventListener('click', () => {
                day.classList.toggle('active');
                const d = parseInt(day.dataset.day);
                if (selectedDays.includes(d)) {
                    selectedDays = selectedDays.filter(x => x !== d);
                } else {
                    selectedDays.push(d);
                }
            });
        });

        // Clock
        function updateClock() {
            const now = new Date();
            document.getElementById('currentTime').textContent = now.toLocaleTimeString('en-GB');
            document.getElementById('currentDate').textContent = now.toLocaleDateString('en-IN', {
                weekday: 'long', year: 'numeric', month: 'long', day: 'numeric'
            });
            checkAlarms(now);
        }
        setInterval(updateClock, 1000);
        updateClock();

        function setAlarm() {
            const time = document.getElementById('alarmTime').value;
            const label = document.getElementById('alarmLabel').value.trim() || "Alarm";

            if (!time) {
                alert("Time select karo!");
                return;
            }

            const alarm = {
                id: Date.now(),
                time: time,
                label: label,
                days: [...selectedDays], // empty = once
                active: true
            };

            alarms.push(alarm);
            save();
            render();
            
            // Reset form
            document.getElementById('alarmTime').value = '';
            document.getElementById('alarmLabel').value = '';
            selectedDays = [];
            document.querySelectorAll('.day').forEach(d => d.classList.remove('active'));
        }

        function render() {
            const list = document.getElementById('alarmsList');
            if (alarms.length === 0) {
                list.innerHTML = '<div class="empty">No alarms yet</div>';
                return;
            }

            const dayNames = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];

            list.innerHTML = alarms.map(a => {
                let daysText = a.days.length === 0 ? "Once" : 
                               a.days.length === 7 ? "Every day" :
                               a.days.map(d => dayNames[d]).join(', ');

                return `
                    <div class="alarm-item">
                        <div>
                            <div class="alarm-time">${format12(a.time)}</div>
                            <div class="alarm-label">${a.label}</div>
                            <div class="alarm-days">${daysText}</div>
                        </div>
                        <button class="delete-btn" onclick="deleteAlarm(${a.id})">×</button>
                    </div>
                `;
            }).join('');
        }

        function format12(t) {
            const [h, m] = t.split(':');
            let hour = parseInt(h);
            const ampm = hour >= 12 ? 'PM' : 'AM';
            hour = hour % 12 || 12;
            return `\( {hour}: \){m} ${ampm}`;
        }

        function deleteAlarm(id) {
            alarms = alarms.filter(a => a.id !== id);
            save();
            render();
        }

        function save() {
            localStorage.setItem('muzaAlarms', JSON.stringify(alarms));
        }

        function checkAlarms(now) {
            const current = now.toTimeString().slice(0, 5);
            const today = now.getDay();

            alarms.forEach(alarm => {
                if (!alarm.active || ringingId) return;

                const matchTime = alarm.time === current;
                const matchDay = alarm.days.length === 0 || alarm.days.includes(today);

                if (matchTime && matchDay) {
                    startRinging(alarm);
                }
            });
        }

        function startRinging(alarm) {
            ringingId = alarm.id;
            document.getElementById('ringingScreen').style.display = 'flex';
            document.getElementById('ringingTime').textContent = format12(alarm.time);
            document.getElementById('ringingLabel').textContent = alarm.label;

            if (navigator.vibrate) {
                navigator.vibrate([600, 200, 600, 200, 600]);
            }
            playSound();
        }

        function playSound() {
            try {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                oscillator = audioCtx.createOscillator();
                const gain = audioCtx.createGain();

                oscillator.type = 'square';
                oscillator.frequency.value = 880;
                gain.gain.value = 0.25;

                oscillator.connect(gain);
                gain.connect(audioCtx.destination);
                oscillator.start();

                soundInterval = setInterval(() => {
                    if (oscillator) {
                        oscillator.frequency.value = oscillator.frequency.value === 880 ? 660 : 880;
                    }
                }, 350);
            } catch (e) {}
        }

        function stopSound() {
            if (oscillator) {
                oscillator.stop();
                oscillator = null;
            }
            if (audioCtx) {
                audioCtx.close();
                audioCtx = null;
            }
            clearInterval(soundInterval);
        }

        function stopAlarm() {
            stopSound();
            document.getElementById('ringingScreen').style.display = 'none';

            const alarm = alarms.find(a => a.id === ringingId);
            if (alarm && alarm.days.length === 0) {
                // One-time alarm → delete
                alarms = alarms.filter(a => a.id !== ringingId);
            }
            save();
            render();
            ringingId = null;
        }

        function snoozeAlarm() {
            stopSound();
            document.getElementById('ringingScreen').style.display = 'none';

            const now = new Date();
            now.setMinutes(now.getMinutes() + 5);
            const newTime = now.toTimeString().slice(0, 5);

            const alarm = alarms.find(a => a.id === ringingId);
            if (alarm) {
                // Temporary one-time snooze alarm
                alarms.push({
                    id: Date.now(),
                    time: newTime,
                    label: alarm.label + " (Snoozed)",
                    days: [],
                    active: true
                });
            }
            save();
            render();
            ringingId = null;
        }

        // Init
        render();
    </script>
</body>
</html>
