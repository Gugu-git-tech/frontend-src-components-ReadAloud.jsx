import React, { useState } from 'react';
import axios from 'axios';

export default function ReadAloud({ text }) {
  const [speed, setSpeed] = useState(1.0);

  async function handleRead() {
    try {
      const resp = await axios.post('/api/tts/speak', { text, speed });
      const url = resp.data.audio_url;
      const audio = new Audio(url);
      audio.playbackRate = speed;
      audio.play();
    } catch (err) {
      alert('TTS failed: ' + (err.response?.data?.error || err.message));
    }
  }

  return (
    <div>
      <h3>Read Aloud</h3>
      <div>
        <label>Speed: <input min="0.5" max="2" step="0.1" value={speed} onChange={e => setSpeed(e.target.value)} /></label>
        <button onClick={handleRead} disabled={!text}>Read Aloud</button>
      </div>
    </div>
  );
}
