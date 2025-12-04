<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pronunciation Practice Game</title>
    <style>
        body { font-family: 'Segoe UI', sans-serif; text-align: center; background-color: #f0f4f8; padding: 20px; }
        .container { max-width: 600px; margin: 0 auto; background: white; padding: 30px; border-radius: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        h1 { color: #2c3e50; }
        .card { background: #e3f2fd; padding: 20px; border-radius: 10px; margin: 20px 0; border: 2px solid #bbdefb; }
        .word { font-size: 1.5em; font-weight: bold; color: #1565c0; display: block; margin-bottom: 10px; }
        .sentence { font-size: 1.2em; font-style: italic; color: #555; }
        
        /* Button Styles */
        button { border: none; padding: 15px 30px; font-size: 1em; border-radius: 50px; cursor: pointer; transition: 0.3s; margin: 5px; }
        button:hover { transform: scale(1.05); }
        button:disabled { background-color: #ccc; cursor: not-allowed; }
        
        .btn-speak { background-color: #4caf50; color: white; font-weight: bold; } /* Green */
        .btn-listen { background-color: #2196f3; color: white; } /* Blue */
        .btn-next { background-color: #607d8b; color: white; font-size: 0.9em; padding: 10px 20px; } /* Grey */

        #result { margin-top: 20px; font-weight: bold; font-size: 1.2em; min-height: 1.5em; }
        .pass { color: green; }
        .fail { color: red; }
        .transcript { color: #888; font-size: 0.9em; margin-top: 5px; }
        .mode-switch { margin-bottom: 20px; display: inline-block; padding: 10px; background: #eee; border-radius: 8px; }
        .switch-label { font-weight: bold; color: #333; margin-left: 5px; }
    </style>
</head>
<body>

<div class="container">
    <h1>🗣️ Speak & Pass</h1>
    
    <div class="mode-switch">
        <input type="checkbox" id="hardModeToggle">
        <label for="hardModeToggle" class="switch-label">🔥 Hard Mode (Whole Sentence)</label>
    </div>

    <p>Read the sentence below clearly.</p>

    <div class="card">
        <span class="word" id="target-word">Loading...</span>
        <span class="sentence" id="target-sentence">...</span>
    </div>

    <button class="btn-listen" onclick="playAudio()">🔊 Listen First</button>
    <br>
    <button id="speak-btn" class="btn-speak" onclick="startListening()">🎤 Tap to Speak</button>

    <div id="result"></div>
    <div id="transcript" class="transcript"></div>
    
    <div style="margin-top:20px;">
        <button class="btn-next" onclick="nextLevel()">Next Sentence ➡️</button>
    </div>
</div>

<script>
    // 1. DATA
    const levels = [
        { word: "Image", sentence: "The image on the screen is very clear." },
        { word: "Image", sentence: "He wants to improve his public image." },
        { word: "Image", sentence: "She took a beautiful image of the sunset." },
        { word: "Change", sentence: "I need to change my clothes." },
        { word: "Change", sentence: "Do you have any spare change?" },
        { word: "Change", sentence: "The weather is going to change soon." },
        { word: "Village", sentence: "He lives in a small village." },
        { word: "Village", sentence: "The whole village came to the party." },
        { word: "Village", sentence: "It is a very quiet village." },
        { word: "Generation", sentence: "This is a new generation of computers." },
        { word: "Generation", sentence: "My grandmother is from a different generation." },
        { word: "Generation", sentence: "We need to think about the future generation." },
        { word: "Strategies", sentence: "We need to develop new strategies." },
        { word: "Strategies", sentence: "What strategies do you use to study?" },
        { word: "Strategies", sentence: "The general discussed his strategies." },
        { word: "Energy", sentence: "I don't have enough energy today." },
        { word: "Energy", sentence: "Solar energy is good for the planet." },
        { word: "Energy", sentence: "Please turn off the light to save energy." },
        { word: "Huge", sentence: "That building is absolutely huge." },
        { word: "Huge", sentence: "They made a huge mistake." },
        { word: "Huge", sentence: "A huge crowd gathered to watch." },
        { word: "Organic", sentence: "I prefer to buy organic vegetables." },
        { word: "Organic", sentence: "Is this milk organic?" },
        { word: "Organic", sentence: "Organic food is often expensive." },
        { word: "Just", sentence: "I just arrived at the station." },
        { word: "Just", sentence: "It was just a misunderstanding." },
        { word: "Just", sentence: "Can you wait just a minute?" },
        { word: "Job", sentence: "She did a great job." },
        { word: "Job", sentence: "I am looking for a new job." },
        { word: "Job", sentence: "It is his job to manage the team." },
        { word: "Angry", sentence: "The customer was very angry." },
        { word: "Angry", sentence: "Please don't get angry with me." },
        { word: "Angry", sentence: "He wrote an angry email." },
        { word: "Large", sentence: "I would like a large coffee." },
        { word: "Large", sentence: "They have a large collection of books." },
        { word: "Large", sentence: "The shirt was too large for him." },
        { word: "Challenge", sentence: "I am ready for a new challenge." },
        { word: "Challenge", sentence: "Learning English is a fun challenge." },
        { word: "Challenge", sentence: "The final exam was a challenge." },
        { word: "Changing", sentence: "The world is changing very fast." },
        { word: "Changing", sentence: "I am changing my mind." },
        { word: "Changing", sentence: "The leaves are changing color." },
        { word: "Psychologist", sentence: "She decided to see a psychologist." },
        { word: "Psychologist", sentence: "The psychologist listened carefully." },
        { word: "Psychologist", sentence: "He wants to be a psychologist." },
        { word: "Live", sentence: "Where do you live currently?" },
        { word: "Live", sentence: "I want to live in a warm country." },
        { word: "Live", sentence: "They live in the house on the corner." },
        { word: "Technology", sentence: "Modern technology makes life easier." },
        { word: "Technology", sentence: "He works in technology." },
        { word: "Technology", sentence: "We use a lot of technology in class." },
        { word: "Chocolate", sentence: "I would love a piece of chocolate." },
        { word: "Chocolate", sentence: "Dark chocolate is my favorite." },
        { word: "Chocolate", sentence: "Don't eat too much chocolate." },
        { word: "Manage", sentence: "How do you manage your time?" },
        { word: "Manage", sentence: "I can manage this task." },
        { word: "Manage", sentence: "It is difficult to manage a team." }
    ];

    // Shuffle Logic
    function shuffleArray(array) {
        for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
        }
    }
    shuffleArray(levels);

    let currentIndex = 0;
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    const recognition = new SpeechRecognition();
    
    // Audio Context for sound effects
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

    recognition.lang = 'en-US'; 
    recognition.interimResults = false;
    recognition.maxAlternatives = 1;

    // Helper: Clean text
    function normalizeText(text) {
        return text.toLowerCase().replace(/[.,\/#!$%\^&\*;:{}=\-_`~()]/g,"").replace(/\s{2,}/g," ").trim();
    }

    // Helper: Play "Ding" (Correct)
    function playPassSound() {
        if (audioCtx.state === 'suspended') audioCtx.resume();
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.connect(gain);
        gain.connect(audioCtx.destination);
        
        osc.type = "sine";
        osc.frequency.setValueAtTime(500, audioCtx.currentTime);
        osc.frequency.exponentialRampToValueAtTime(1000, audioCtx.currentTime + 0.1);
        
        gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + 0.5);
        
        osc.start();
        osc.stop(audioCtx.currentTime + 0.5);
    }

    // Helper: Play "Bonk" (Wrong)
    function playFailSound() {
        if (audioCtx.state === 'suspended') audioCtx.resume();
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.connect(gain);
        gain.connect(audioCtx.destination);
        
        osc.type = "sawtooth";
        osc.frequency.setValueAtTime(150, audioCtx.currentTime);
        osc.frequency.linearRampToValueAtTime(100, audioCtx.currentTime + 0.3);
        
        gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
        gain.gain.linearRampToValueAtTime(0.0001, audioCtx.currentTime + 0.3);
        
        osc.start();
        osc.stop(audioCtx.currentTime + 0.3);
    }

    // Play Target Sentence
    function playAudio() {
        window.speechSynthesis.cancel();
        const sentenceToRead = levels[currentIndex].sentence;
        const utterance = new SpeechSynthesisUtterance(sentenceToRead);
        utterance.lang = 'en-US';
        utterance.rate = 0.9;
        window.speechSynthesis.speak(utterance);
    }

    function updateUI() {
        window.speechSynthesis.cancel();
        if (currentIndex >= levels.length) {
             document.querySelector('.container').innerHTML = `
                <h1>🎉 You Finished!</h1>
                <p>Great practice session.</p>
                <button onclick="location.reload()" class="btn-listen">Play Again 🔄</button>
             `;
             return;
        }
        document.getElementById('target-word').innerText = levels[currentIndex].word;
        document.getElementById('target-sentence').innerText = `"${levels[currentIndex].sentence}"`;
        document.getElementById('result').innerText = "";
        document.getElementById('transcript').innerText = "";
        document.getElementById('speak-btn').disabled = false;
        document.getElementById('speak-btn').innerText = "🎤 Tap to Speak";
    }

    function startListening() {
        window.speechSynthesis.cancel();
        // Resume Audio Context on user gesture to ensure sounds play on mobile
        if (audioCtx.state === 'suspended') audioCtx.resume();
        
        const btn = document.getElementById('speak-btn');
        btn.disabled = true;
        btn.innerText = "Listening... 👂";
        recognition.start();
    }

    recognition.onresult = (event) => {
        const speechResult = event.results[0][0].transcript;
        const isHardMode = document.getElementById('hardModeToggle').checked;
        const cleanSpeech = normalizeText(speechResult);
        const cleanTargetWord = normalizeText(levels[currentIndex].word);
        const cleanTargetSentence = normalizeText(levels[currentIndex].sentence);
        
        document.getElementById('transcript').innerText = `I heard: "${speechResult}"`;
        
        let passed = false;
        if (isHardMode) {
            if (cleanSpeech === cleanTargetSentence) passed = true;
        } else {
            if (cleanSpeech.includes(cleanTargetWord)) passed = true;
        }

        if (passed) {
            playPassSound(); // DING!
            document.getElementById('result').innerHTML = "<span class='pass'>✅ PASS! Excellent.</span>";
        } else {
            playFailSound(); // BONK!
            document.getElementById('result').innerHTML = isHardMode ? "<span class='fail'>❌ Try Again. (Must match sentence exactly!)</span>" : "<span class='fail'>❌ Try Again. Keep it clear!</span>";
        }
        document.getElementById('speak-btn').disabled = false;
        document.getElementById('speak-btn').innerText = "🎤 Tap to Speak";
    };

    recognition.onspeechend = () => { recognition.stop(); };
    recognition.onerror = (event) => {
        document.getElementById('speak-btn').disabled = false;
        document.getElementById('speak-btn').innerText = "🎤 Tap to Speak";
        document.getElementById('result').innerText = "Error: " + event.error;
    };

    function nextLevel() {
        currentIndex++;
        updateUI();
    }

    updateUI();
</script>
</body>
</html>
