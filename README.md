<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Siri-like Personal Voice Assistant</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin-top: 50px;
    }
    button {
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
    }
    #output {
      margin-top: 20px;
      font-size: 18px;
    }
  </style>
</head>
<body>
  <h1>Siri-like Personal Voice Assistant</h1>
  <button onclick="startListening()">Activate Assistant</button>
  <p id="output"></p>

  <script>
    const output = document.getElementById('output');

    // Speak function using speech synthesis
    function speak(text) {
      const utterance = new SpeechSynthesisUtterance(text);
      window.speechSynthesis.speak(utterance);
    }

    // Start listening for voice commands
    function startListening() {
      const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
      recognition.lang = 'en-US';
      recognition.interimResults = false;
      recognition.maxAlternatives = 1;

      speak("Hello, I am your voice assistant. How can I help you?");
      recognition.start();

      recognition.onresult = (event) => {
        const command = event.results[0][0].transcript.toLowerCase();
        output.textContent = `You said: "${command}"`;
        handleCommand(command);
      };

      recognition.onerror = () => {
        speak("Sorry, I couldn't hear you. Please try again.");
      };
    }

    // Handle voice commands
    function handleCommand(command) {
      if (command.includes('time')) {
        const currentTime = new Date().toLocaleTimeString();
        speak(`The current time is ${currentTime}`);
      } else if (command.includes('weather')) {
        speak("Opening the weather forecast.");
        window.open('https://www.weather.com');
      } else if (command.includes('search for')) {
        const query = command.replace('search for', '').trim();
        speak(`Searching Google for ${query}`);
        window.open(`https://www.google.com/search?q=${query}`);
      } else if (command.includes('open youtube')) {
        speak("Opening YouTube.");
        window.open('https://www.youtube.com');
      } else if (command.includes('your name')) {
        speak("I am your personal assistant, similar to Siri.");
      } else if (command.includes('goodbye')) {
        speak("Goodbye! Have a wonderful day.");
      } else {
        speak("I'm not sure how to help with that.");
      }
    }
  </script>
</body>
</html>