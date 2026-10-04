<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Muza Alarm</title>
    <style>
        :root {
            --bg: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
            --card: rgba(255,255,255,0.09);
            --text: #fff;
            --input: rgba(255,255,255,0.12);
            --primary: linear-gradient(to right, #ff6b6b, #ee5a24);
        }
        body.light {
            --bg: linear-gradient(135deg, #e0eafc, #cfdef3);
            --card: rgba(255,255,255,0.9);
            --text: #1a1a1a;
            --input: rgba(0,0,0,0.07);
        }

        * { margin:0; padding:0; box-sizing:border-box; font-family: system-ui, -apple-system, sans-serif; }
        body {
            background: var(--bg);
            min-height: 100vh;
            color: var(--text);
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 16px;
            transition: 0.3s;
        }

        .header {
            width: 100%;
            max-width: 440px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
        }
        h1 {
            font-size: 1.8rem;
            background: linear-gradient(to right, #ff6b6b, #feca57, #48dbfb);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .theme-btn {
            background: var(--card);
            border: none;
            width: 42px;
            height: 42px;
            border-radius: 50%;
            font-size: 1.25rem;
            cursor: pointer;
            color: var(--text);
        }

        .clock {
            font-size: 3.6rem;
            font-weight: 700;
            letter-spacing: 2px;
            margin: 10px 0 2px;
        }
        .date {
            font-size: 1rem;
            opacity: 0.75;
            margin-bottom: 20px;
        }

        .container {
            background: var(--card);
            backdrop-filter: blur(16px);
            border-radius: 22px;
            padding: 22px;
            width: 100%;
            max-width: 440px;
            box-shadow: 0 12px 30px rgba(0,0,0,0.25);
        }

        .form-group { margin-bottom: 14px; }
        label {
            display: block;
            margin-bottom: 5px;
            font-size: 0.9rem;
            opacity: 0.9;
        }
        input[type="time"], input[type="text"], select {
            width: 100%;
            padding: 12px 14px;
            border: none;
            border-radius: 12px;
            background: var(--input);
            color: var(--text);
            font-size: 1.05rem;
            outline: none;
        }

        .days {
            display: flex;
            gap: 6px;
            flex-wrap: wrap;
            margin-top: 4px;
        }
        .day {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.78rem;
            font-weight: 600;
            background: var(--input);
            cursor: pointer;
            user-select: none;
            transition: 0.2s;
        }
        .day.active {
            background: #ff6b6b;
            color: white;
        }

        .btn-primary {
            width: 100%;
            padding: 14px;
            border: none;
            border-radius: 14px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            background: var(--primary);
            color: white;
            margin-top: 6px;
        }

        .alarms-list { margin-top: 22px; }

        .alarm-item {
            background: var(--input);
            border-radius: 16px;
            padding: 13px 15px;
            margin-bottom: 11px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            animation: slideIn 0.3s ease;
        }
        @keyframes slideIn {
            from { opacity: 0; transform: translateY(12px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .alarm-info { flex: 1; }
        .alarm-time {
            font-size: 1.45rem;
            font-weight: 700;
        }
        .alarm-label {
            font-size: 0.85rem;
            opacity: 0.8;
            margin-top: 1px;
        }
        .alarm-days {
            font-size: 0.72rem;
            opacity: 0.65;
            margin-top: 3px;
        }

        .alarm-actions {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        /* Toggle Switch */
        .switch {
            position: relative;
            width: 46px;
            height: 26px;
        }
        .switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }
        .slider {
            position: absolute;
            cursor: pointer;
            inset: 0;
            background: #555;
            border-radius: 26px;
            transition: 0.3s;
        }
        .slider:before {
            position: absolute;
            content: "";
            height: 20px;
            width: 20px;
            left: 3px;
            bottom: 3px;
            background: white;
            border-radius: 50%;
            transition: 0.3s;
        }
        input:checked + .slider {
            background: #2ed573;
        }
        input:checked + .slider:before {
            transform: translateX(20px);
        }

        .delete-btn {
            background: #ff4757;
            color: white;
            border: none;
            width: 34px;
            height: 34px;
            border-radius: 50%;
            font-size: 1.2rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .empty {
            text-align: center;
            opacity: 0.55;
            padding: 25px 10px;
            font-size: 0.95rem;
        }

        /* Ringing Screen */
        .ringing {
            position: fixed;
            inset: 0;
            background: rgba(0,0,0,0.95);
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 999;
            animation: pulseBg 1.1s infinite;
        }
        @keyframes pulseBg {
            0%,100% { background: rgba(180,20,20,0.95); }
            50% { background: rgba(0,0,0,0.97); }
        }
        .ringing h2 {
            font-size: 2.6rem;
            animation: shake 0.4s infinite;
        }
        @keyframes shake {
            0%,100% { transform: translateX(0); }
            25% { transform: translateX(-10px); }
            75% { transform: translateX(10px); }
        }
        .big-time {
            font-size: 4.2rem;
            font-weight: 800;
            margin: 10px 0;
        }
        .ring-label {
            font-size: 1.3rem;
            margin-bottom: 30px;
            opacity: 0.9;
        }
        .ring-btns {
            display: flex;
            gap: 12px;
            flex-wrap: wrap;
            justify-content: center;
        }
        .ring-btns button {
            padding: 15px 26px;
            border: none;
            border-radius: 14px;
            font-size: 1.05rem;
            font-weight: 600;
            cursor: pointer;
            min-width: 130px;
        }
        .snooze-btn { background: #feca57; color: #222; }
        .stop-btn { background: #ff4757; color: white; }
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
            <label>Snooze Time</label>
            <select id="snoozeTime">
                <option value="5">5 minutes</option>
                <option value="10">10 minutes</option>
                <option value="15">15 minutes</option>
            </select>
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

    <!-- Ringing Screen -->
    <div class="ringing" id="ringingScreen">
        <h2>WAKE UP!</h2>
        <div class="big-time" id="ringingTime"></div>
        <div class="ring-label" id="ringingLabel"></div>
        <div class="ring-btns">
            <button class="snooze-btn" id="snoozeBtn" onclick="snoozeAlarm()">Snooze</button>
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
        let currentSnooze = 5;

        // Theme
        function toggleTheme() {
            document.body.classList.toggle('light');
            localStorage.setItem('theme', document.body.classList.contains('light') ? 'light' : 'dark');
        }
        if (localStorage.getItem('theme') === 'light') document.body.classList.add('light');

        // Days
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
            currentSnooze = parseInt(document.getElementById('snoozeTime').value);

            if (!time) {
                alert("Time select karo!");
                return;
            }

            alarms.push({
                id: Date.now(),
                time: time,
                label: label,
                days: [...selectedDays],
                active: true,
                snooze: currentSnooze
            });

            save();
            render();

            // Reset
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

            const dayNames = ['Sun','Mon','Tue','Wed','Thu','Fri','Sat'];

            list.innerHTML = alarms.map(a => {
                let daysText = a.days.length === 0 ? "Once" :
                               a.days.length === 7 ? "Every day" :
                               a.days.map(d => dayNames[d]).join(', ');

                return `
                <div class="alarm-item">
                    <div class="alarm-info">
                        <div class="alarm-time">${format12(a.time)}</div>
                        <div class="alarm-label">${a.label}</div>
                        <div class="alarm-days">${daysText} • Snooze ${a.snooze}m</div>
                    </div>
                    <div class="alarm-actions">
                        <label class="switch">
                            <input type="checkbox" \( {a.active ? 'checked' : ''} onchange="toggleAlarm( \){a.id})">
                            <span class="slider"></span>
                        </label>
                        <button class="delete-btn" onclick="deleteAlarm(${a.id})">×</button>
                    </div>
                </div>`;
            }).join('');
        }

        function format12(t) {
            const [h, m] = t.split(':');
            let hour = parseInt(h);
            const ampm = hour >= 12 ? 'PM' : 'AM';
            hour = hour % 12 || 12;
            return `\( {hour}: \){m} ${ampm}`;
        }

        function toggleAlarm(id) {
            const alarm = alarms.find(a => a.id === id);
            if (alarm) {
                alarm.active = !alarm.active;
                save();
                render();
            }
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
            currentSnooze = alarm.snooze || 5;
            document.getElementById('ringingScreen').style.display = 'flex';
            document.getElementById('ringingTime').textContent = format12(alarm.time);
            document.getElementById('ringingLabel').textContent = alarm.label;
            document.getElementById('snoozeBtn').textContent = `Snooze ${currentSnooze} min`;

            if (navigator.vibrate) navigator.vibrate([700, 200, 700, 200, 700]);
            playSound();
        }

        function playSound() {
            try {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                oscillator = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                oscillator.type = 'square';
                oscillator.frequency.value = 880;
                gain.gain.value = 0.3;
                oscillator.connect(gain);
                gain.connect(audioCtx.destination);
                oscillator.start();

                soundInterval = setInterval(() => {
                    if (oscillator) {
                        oscillator.frequency.value = oscillator.frequency.value === 880 ? 660 : 880;
                    }
                }, 320);
            } catch(e) {}
        }

        function stopSound() {
            if (oscillator) { oscillator.stop(); oscillator = null; }
            if (audioCtx) { audioCtx.close(); audioCtx = null; }
            clearInterval(soundInterval);
        }

        function stopAlarm() {
            stopSound();
            document.getElementById('ringingScreen').style.display = 'none';

            const alarm = alarms.find(a => a.id === ringingId);
            if (alarm && alarm.days.length === 0) {
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
            now.setMinutes(now.getMinutes() + currentSnooze);
            const newTime = now.toTimeString().slice(0, 5);

            const original = alarms.find(a => a.id === ringingId);
            alarms.push({
                id: Date.now(),
                time: newTime,
                label: (original ? original.label : "Alarm") + " (Snoozed)",
                days: [],
                active: true,
                snooze: currentSnooze
            });

            save();
            render();
            ringingId = null;
        }

        // Init
        render();
    </script>
</body>
</html>
