<script>
    let audioFile = null;
    let transcription = '';
    let loading = false;
  
    async function handleTranscribe() {
      if (!audioFile) {
        alert('Please select an audio file.');
        return;
      }
  
      // Prepare FormData for the API request
      const formData = new FormData();
      formData.append('file', audioFile); // Attach the audio file directly
      formData.append('language', 'english'); // Adjust based on LemonFox API’s accepted languages, if needed
      formData.append('response_format', 'json'); // Check if this is the correct parameter for LemonFox
  
      try {
        loading = true;
        const response = await fetch('https://api.lemonfox.ai/v1/audio/transcriptions', {
          method: 'POST',
          headers: {
            Authorization: `Bearer vUkPCQ6QQRNaOjlFs1TBT9fEyxBBlK3E`, // Replace with your LemonFox API key
            // Do NOT add Content-Type manually, as the FormData will set it correctly
          },
          body: formData,
        });
  
        if (!response.ok) {
          throw new Error(`Error in transcription request: ${response.status}`);
        }
  
        const result = await response.json();
        transcription = result.text || 'Error in transcription';
      } catch (error) {
        transcription = 'Failed to transcribe audio. Try again.';
        console.error(error); // Log the error for debugging
      } finally {
        loading = false;
      }
    }
  
    function handleFileChange(event) {
      audioFile = event.target.files[0];
    }
  </script>
  
  <style>
    .loading { font-style: italic; color: gray; }
  </style>
  
  <div>
    <h2>Audio to Text Converter</h2>
    <input type="file" accept="audio/*" on:change="{handleFileChange}" />
    <button on:click="{handleTranscribe}">Transcribe Audio</button>
  
    {#if loading}
      <p class="loading">Transcribing...</p>
    {/if}
  
    {#if transcription}
      <h3>Transcription Result:</h3>
      <p>{transcription}</p>
    {/if}
  </div>
  