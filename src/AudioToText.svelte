<script>
    let audioFile = null;
    let transcription = '';
    let loading = false;
  
    async function handleTranscribe() {
      if (!audioFile) {
        alert('Please select an audio file.');
        return;
      }
  
      const formData = new FormData();
      formData.append('file', audioFile);
      formData.append('language', 'english');
      formData.append('response_format', 'json');
  
      try {
        loading = true;
        const response = await fetch('https://api.lemonfox.ai/v1/audio/transcriptions', {
          method: 'POST',
          headers: {
            Authorization: `Bearer vUkPCQ6QQRNaOjlFs1TBT9fEyxBBlK3E`, // Using environment variable
          },
          body: formData,
        });
  
        if (!response.ok) {
          throw new Error('Error in transcription request');
        }
  
        const result = await response.json();
        transcription = result.text || 'Error in transcription';
      } catch (error) {
        transcription = 'Failed to transcribe audio. Try again.';
        console.error(error);
      } finally {
        loading = false;
      }
    }
  
    function handleFileChange(event) {
      audioFile = event.target.files[0];
    }
  </script>
  
  <style>
    /* Basic Reset */
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
  
    /* Container Styles */
    .container {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 1rem;
      width: 100%;
      max-width: 600px;
      margin: auto;
    }
  
    h2 {
      font-size: 1.5rem;
      margin-bottom: 1rem;
      text-align: center;
    }
  
    /* Input Styles */
    input[type="file"] {
      width: 100%;
      margin: 1rem 0;
      padding: 0.5rem;
      font-size: 1rem;
    }
  
    /* Button Styles */
    button {
      background-color: #007bff;
      color: white;
      border: none;
      padding: 0.8rem 1.5rem;
      font-size: 1rem;
      border-radius: 5px;
      cursor: pointer;
      transition: background-color 0.3s ease;
      width: 100%;
    }
  
    button:hover {
      background-color: #0056b3;
    }
  
    /* Loading Text */
    .loading {
      font-style: italic;
      color: gray;
      margin-top: 1rem;
    }
  
    /* Transcription Result */
    .transcription-result {
      margin-top: 1.5rem;
      padding: 1rem;
      background-color: #f9f9f9;
      border-radius: 5px;
      font-size: 1rem;
      width: 100%;
      max-width: 600px;
      word-wrap: break-word;
    }
  
    /* Responsive Styling */
    @media (min-width: 768px) {
      .container {
        padding: 2rem;
      }
  
      h2 {
        font-size: 2rem;
      }
  
      button {
        width: auto;
      }
  
      .transcription-result {
        font-size: 1.1rem;
      }
    }
  </style>
  
  <div class="container">
    <h2>Audio to Text Converter</h2>
    <!-- Updated input to include capture attribute for mobile -->
    <input type="file" accept="audio/*"  on:change="{handleFileChange}" />
    <button on:click="{handleTranscribe}">Transcribe Audio</button>
  
    {#if loading}
      <p class="loading">Transcribing...</p>
    {/if}
  
    {#if transcription}
      <div class="transcription-result">
        <h3>Transcription Result:</h3>
        <p>{transcription}</p>
      </div>
    {/if}
  </div>
  