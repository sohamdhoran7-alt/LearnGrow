<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Muza Alarm App</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', system-ui, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
            min-height: 100vh;
            color: white;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        h1 {
            font-size: 2.5rem;
            margin: 20px 0;
            background: linear-gradient(to right, #ff6b6b, #feca57, #48dbfb);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-align: center;
        }

        .clock {
            font-size: 4.5rem;
            font-weight: 700;
            letter-spacing: 4px;
            margin: 20px 0;
            text-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
        }

        .date {
            font-size: 1.3rem;
            opacity: 0.8;
            margin-bottom: 30px;
        }

        .container {
            background: rgba(255, 255, 255, 0.08);
            backdrop-filter: blur(12px);
            border-radius: 24px;
            padding: 30px;
            width: 100%;
            max-width: 420px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .form-group {
            margin-bottom: 18px;
        }

        label {
            display: block;
            margin-bottom: 6px;
            font-size: 0.95rem;
            opacity: 0.9;
        }

        input, select {
            width: 100%;
            padding: 14px;
            border: none;
            border-radius: 12px;
            background: rgba(255, 255, 255, 0.12);
            color: white;
            font-size: 1.1rem;
            outline: none;
        }

        input::placeholder {
            color: rgba(255, 255, 255, 0.5);
        }

        button {
            width: 100%;
            padding: 16px;
            border: none;
            border-radius: 14px;
            font-size: 1.15rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-top: 10px;
        }

        .btn-primary {
            background: linear-gradient(to right, #ff6b6b, #ee5a24);
            color: white;
        }

        .btn-primary:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 20px rgba(238, 90, 36, 0.4);
        }

        .alarms-list {
            margin-top: 30px;
        }

        .alarm-item {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 16px;
            padding: 16px 20px;
            margin-bottom: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            animation: slideIn 0.4s ease;
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .alarm-time {
            font-size: 1.6rem;
            font-weight: 700;
        }

        .alarm-label {
            font-size: 0.9rem;
            opacity: 0.8;
        }

        .delete-btn {
            background: #ff4757;
            color: white;
            border: none;
            width: 36px;
            height: 36px;
            border-radius: 50%;
            font-size: 1.2rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* Alarm Ringing Overlay */
        .ringing {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.92);
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            animation: pulse 1s infinite;
        }

        @keyframes pulse {
            0% { background: rgba(255, 50, 50, 0.9); }
            50% { background: rgba(0, 0, 0, 0.95); }
            100% { background: rgba(255, 50, 50, 0.9); }
        }

        .ringing h2 {
            font-size: 3rem;
            margin-bottom: 10px;
            animation: shake 0.5s infinite;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-15px); }
            75% { transform: translateX(15px); }
        }

        .ringing .time {
            font-size: 5rem;
            font-weight: 800;
            margin: 20px 0;
        }

        .snooze-btn, .stop-btn {
            width: 180px;
            margin: 10px;
            padding: 18px;
            font-size: 1.2rem;
        }

        .snooze-btn {
            background: #feca57;
            color: #333;
        }

        .stop-btn {
            background: #ff4757;
            color: white;
        }

        .empty {
            text-align: center;
            opacity: 0.6;
            padding: 30px;
        }
    </style>
</head>
<body>
    <h1>Muza Alarm</h1>
    
    <div class="clock" id="currentTime">00:00:00</div>
    <div class="date" id="currentDate"></div>

    <div class="container">
        <div class="form-group">
            <label>Alarm Time</label>
            <input type="time" id="alarmTime" required>
        </div>

        <div class="form-group">
            <label>Label (optional)</label>
            <input type="text" id="alarmLabel" placeholder="Utho bhai... 😂">
        </div>

        <button class="btn-primary" onclick="setAlarm()">Set Alarm</button>

        <div class="alarms-list" id="alarmsList">
            <div class="empty">No alarms set yet</div>
        </div>
    </div>

    <!-- Ringing Screen -->
    <div class="ringing" id="ringingScreen">
        <h2>WAKE UP!</h2>
        <div class="time" id="ringingTime"></div>
        <div id="ringingLabel" style="font-size: 1.5rem; margin-bottom: 30px;"></div>
        
        <button class="snooze-btn" onclick="snoozeAlarm()">Snooze 5 min</button>
        <button class="stop-btn" onclick="stopAlarm()">Stop Alarm</button>
    </div>

    <script>
        let alarms = JSON.parse(localStorage.getItem('muzaAlarms')) || [];
        let ringingAlarmId = null;
        let audioContext = null;
        let oscillator = null;

        // Update Clock every second
        function updateClock() {
            const now = new Date();
            const timeStr = now.toLocaleTimeString('en-US', { hour12: false });
            document.getElementById('currentTime').textContent = timeStr;
            
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            document.getElementById('currentDate').textContent = now.toLocaleDateString('en-IN', options);

            checkAlarms(now);
        }

        setInterval(updateClock, 1000);
        updateClock();

        // Set new alarm
        function setAlarm() {
            const timeInput = document.getElementById('alarmTime').value;
            const label = document.getElementById('alarmLabel').value || "Alarm";

            if (!timeInput) {
                alert("Pehle time select karo bhai!");
                return;
            }

            const alarm = {
                id: Date.now(),
                time: timeInput,
                label: label,
                active: true
            };

            alarms.push(alarm);
            saveAlarms();
            renderAlarms();
            
            // Clear inputs
            document.getElementById('alarmTime').value = '';
            document.getElementById('alarmLabel').value = '';
        }

        // Render all alarms
        function renderAlarms() {
            const list = document.getElementById('alarmsList');
            
            if (alarms.length === 0) {
                list.innerHTML = '<div class="empty">No alarms set yet</div>';
                return;
            }

            list.innerHTML = alarms.map(alarm => `
                <div class="alarm-item">
                    <div>
                        <div class="alarm-time">${formatTime(alarm.time)}</div>
                        <div class="alarm-label">${alarm.label}</div>
                    </div>
                    <button class="delete-btn" onclick="deleteAlarm(${alarm.id})">×</button>
                </div>
            `).join('');
        }

        function formatTime(time24) {
            const [h, m] = time24.split(':');
            const hour = parseInt(h);
            const ampm = hour >= 12 ? 'PM' : 'AM';
            const hour12 = hour % 12 || 12;
            return `\( {hour12}: \){m} ${ampm}`;
        }

        function deleteAlarm(id) {
            alarms = alarms.filter(a => a.id !== id);
            saveAlarms();
            renderAlarms();
        }

        function saveAlarms() {
            localStorage.setItem('muzaAlarms', JSON.stringify(alarms));
        }

        // Check if any alarm should ring
        function checkAlarms(now) {
            const currentTime = now.toTimeString().slice(0, 5); // HH:MM

            alarms.forEach(alarm => {
                if (alarm.active && alarm.time === currentTime && !ringingAlarmId) {
                    startRinging(alarm);
                }
            });
        }

        // Start ringing
        function startRinging(alarm) {
            ringingAlarmId = alarm.id;
            document.getElementById('ringingScreen').style.display = 'flex';
            document.getElementById('ringingTime').textContent = formatTime(alarm.time);
            document.getElementById('ringingLabel').textContent = alarm.label;

            // Vibrate if supported
            if (navigator.vibrate) {
                navigator.vibrate([500, 200, 500, 200, 500]);
            }

            // Play sound
            playAlarmSound();
        }

        function playAlarmSound() {
            try {
                audioContext = new (window.AudioContext || window.webkitAudioContext)();
                oscillator = audioContext.createOscillator();
                const gainNode = audioContext.createGain();

                oscillator.type = 'square';
                oscillator.frequency.setValueAtTime(800, audioContext.currentTime);
                gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);

                oscillator.connect(gainNode);
                gainNode.connect(audioContext.destination);

                oscillator.start();
                
                // Beep pattern
                setInterval(() => {
                    if (oscillator) {
                        oscillator.frequency.setValueAtTime(
                            oscillator.frequency.value === 800 ? 600 : 800, 
                            audioContext.currentTime
                        );
                    }
                }, 400);
            } catch (e) {
                console.log("Audio not supported");
            }
        }

        function stopSound() {
            if (oscillator) {
                oscillator.stop();
                oscillator = null;
            }
            if (audioContext) {
                audioContext.close();
                audioContext = null;
            }
        }

        function stopAlarm() {
            stopSound();
            document.getElementById('ringingScreen').style.display = 'none';
            
            // Remove the alarm after it rings (one-time)
            alarms = alarms.filter(a => a.id !== ringingAlarmId);
            saveAlarms();
            renderAlarms();
            
            ringingAlarmId = null;
        }

        function snoozeAlarm() {
            stopSound();
            document.getElementById('ringingScreen').style.display = 'none';

            // Snooze for 5 minutes
            const now = new Date();
            now.setMinutes(now.getMinutes() + 5);
            
            const hours = String(now.getHours()).padStart(2, '0');
            const mins = String(now.getMinutes()).padStart(2, '0');
            const newTime = `\( {hours}: \){mins}`;

            // Update the existing alarm time
            const alarm = alarms.find(a => a.id === ringingAlarmId);
            if (alarm) {
                alarm.time = newTime;
                alarm.label = alarm.label + " (Snoozed)";
            }

            saveAlarms();
            renderAlarms();
            ringingAlarmId = null;
        }

        // Initial render
        renderAlarms();
    </script>
</body>
</html>
