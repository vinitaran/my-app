<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  let day = '';
  let month = '';
  let year = '';
  let keyCount = 0;
  let backspaceCount = 0;
  let startTime = null;
  let elapsedTime = 0;
  let timerInterval = null;
  let timeTaken = null;
  let averageTypingSpeed = 0;

  const startTimer = () => {
    startTime = Date.now();
    timerInterval = setInterval(updateTime, 100);
  };

  const stopTimer = () => {
    clearInterval(timerInterval);
    elapsedTime = Date.now() - startTime;
  };

  const updateTime = () => {
    elapsedTime = Date.now() - startTime;
  };

  const handleInputClick = () => {
    startTimer();
  };

  const handleSubmit = () => {
    const modal = document.getElementById('notificationModal');
    modal.style.display = 'block';
    stopTimer();
    timeTaken = elapsedTime;
    if (timeTaken && keyCount > 0) {
      averageTypingSpeed = (keyCount + backspaceCount) / (timeTaken / 1000);
    }
    const date = `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`;
    dispatch('submit', { date });
  };

  const handleKeyDown = (event) => {
    const allowedKeys = [46, 8, 9, 27, 13, 110, 190];
    const ctrlKeys = [65, 67, 86, 88];
    const isCtrlPressed = event.ctrlKey || event.metaKey;
    const isSpecialKey = allowedKeys.includes(event.keyCode) || (ctrlKeys.includes(event.keyCode) && isCtrlPressed);
    const isNavigationKey = (event.keyCode >= 35 && event.keyCode <= 40);
    const isNumericInput = (!event.shiftKey && (event.keyCode >= 48 && event.keyCode <= 57)) || (event.keyCode >= 96 && event.keyCode <= 105);

    if (event.keyCode === 8) { // Backspace
      backspaceCount++;
    }

    if (!(isSpecialKey || isNavigationKey || isNumericInput)) {
      event.preventDefault();
    } else {
      keyCount++;
    }
  };

  const navigateToVariantB = () => {
    console.log('Navigating to Variant B...');
    window.location.href = '/variant-b';
    console.log('Navigation complete.');
  };
</script>

<div class="text-center mt-5">
  <h1>Enter your Birthday</h1>

  <div class="form-group">
    <div class="date-picker form-inline">
      <div class="form-group">
        <label for="day">Day</label>
        <input 
          bind:value={day} 
          id="day" 
          class="date-picker--day" 
          type="number" 
          min="1" 
          max="31" 
          placeholder="dd" 
          maxlength="2" 
          on:click={handleInputClick}
          on:keydown={handleKeyDown}
        />
      </div>
      <div class="form-group">
        <label for="month">Month</label>
        <input 
          bind:value={month} 
          id="month" 
          class="date-picker--month" 
          type="number" 
          min="1" 
          max="12" 
          placeholder="mm" 
          maxlength="2" 
          on:click={handleInputClick}
          on:keydown={handleKeyDown}
        />
      </div>
      <div class="form-group">
        <label for="year">Year</label>
        <input 
          bind:value={year} 
          id="year" 
          class="date-picker--year" 
          type="number" 
          placeholder="yyyy" 
          maxlength="4" 
          on:click={handleInputClick}
          on:keydown={handleKeyDown}
        />
      </div>
    </div>
  </div>

  <!-- Notification Modal -->
  <div class="notification-modal" id="notificationModal">
    <div class="notification-content">
      <h2>Take a Screenshot!</h2>
      <p>Please take a screenshot of your submission. Click below to proceed to Variant B.</p>
      <button class="btn btn-primary" on:click={navigateToVariantB}>Proceed to Variant B</button>
    </div>
  </div>

  <div>
    Key Count: {keyCount}
  </div>
  <div>
    Backspace Count: {backspaceCount}
  </div>
  <div>
    Time Taken: {(timeTaken / 1000).toFixed(2)} seconds
  </div>
  <div>
    Average Typing Speed: {averageTypingSpeed.toFixed(2)} keys per second
  </div>

  <button class="btn btn-primary mt-3" on:click={handleSubmit}>Submit</button>
</div>

<style>
  .text-center {
    text-align: center;
  }

  .form-group {
    margin-bottom: 20px; /* Add space below each form group */
  }

  .date-picker {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .form-group label {
    margin-right: 10px;
  }

  .form-group input {
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 5px;
    width: 50px;
    text-align: center;
  }

  .form-group input[type="number"]::-webkit-inner-spin-button,
  .form-group input[type="number"]::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  .btn {
    padding: 10px 20px;
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    margin-top: 20px; /* Add space above the button */
    margin-bottom: 20px; /* Add space below the button */
  }

  .btn:hover {
    background-color: #0056b3;
  }
</style>
