<script>
  import { onMount } from 'svelte';

  let dateInput = '';
  let dateFormat = 'DD/MM/YYYY';
  let isValidDate = false;
  let errorSummary = '';
  let keyCount = 0;
  let backspaceCount = 0;
  let startTime = 0;
  let endTime = 0;
  let timeTaken = 0;
  let averageSpeed = 0;

  let isBackspacePressed = false;
  let isTimeStarted = false;

     const navigateToVariantB = () =>  {
    console.log('Navigating to Variant B...');
    window.location.href = 'https://docs.google.com/forms/d/e/1FAIpQLSfN5KSoGmuhVcXFXDH3oUEPKHdj0gL-akc3NcKcvvXsCcY0-w/viewform';
    console.log('Navigation complete.');
}

  const startTimer = () => {
    startTime = Date.now();
    isTimeStarted = true;
  };

  const stopTimer = () => {
    endTime = Date.now();
    isTimeStarted = false;
  };

  const autoFormatDate = (input) => {
    let preFormattedInput = input;

    if (input && !isBackspacePressed) {
      preFormattedInput = preFormattedInput.replaceAll('/', '');

      if (preFormattedInput.length > 2) {
        preFormattedInput =
          preFormattedInput.slice(0, 2) +
          '/' +
          preFormattedInput.slice(2, 4) +
          '/' +
          preFormattedInput.slice(4, 8);
      } else if (preFormattedInput.length > 4) {
        preFormattedInput = preFormattedInput.slice(0, 4) + '/' + preFormattedInput.slice(4, 8);
      }
    }

    dateInput = preFormattedInput;
  };

  const onDateInputChange = (event) => {
    const value = event.target.value;
    autoFormatDate(value);
    if (!isTimeStarted) startTimer();
  };

  const validateDate = () => {
    const modal = document.getElementById('notificationModal');
  modal.style.display = 'block';
    if (dateInput) {
      const [day, month, year] = dateInput.split('/');
      const formattedDate = new Date(`${year}-${month}-${day}`);

      isValidDate = !isNaN(formattedDate.getDate()) && formattedDate.getFullYear() == year;

    } else {
      isValidDate = false;
      errorSummary = '';
    }
    stopTimer();
    calculateSpeed();
  };

  const keyDown = (event) => {
    const keyCode = event.keyCode || event.which;
    isBackspacePressed = keyCode === 8;
    if (keyCode === 8) backspaceCount++;
    keyCount++;
  };

  const calculateSpeed = () => {
    timeTaken = endTime - startTime;
    if (timeTaken && keyCount > 0) {
      averageSpeed = keyCount / (timeTaken / 1000); // keys per second
    }
  };

  onMount(() => {
    // Initialize your component here if needed
  });
</script>

<div class="container">
  <h1>Enter your Birthday</h1>
  <input
    id="dateInput"
    class="date-input"
    type="text"
    placeholder={dateFormat}
    name="custom date input"
    bind:value={dateInput}
    maxlength={dateFormat.length}
    on:input={onDateInputChange}
    on:keydown={keyDown}
  />



  <!-- Notification Modal -->
<div class="notification-modal" id="notificationModal">
  <div class="notification-content">
    <h2>Take a Screenshot!</h2>
    <p>Please take a screenshot of your submission. Click below to proceed.</p>
    <button class="btn btn-primary" on:click={navigateToVariantB}>Proceed</button>
  </div>
</div>

  <div class:error-summary={isValidDate}>
    {errorSummary}
  </div>
  <div>
    Key Count: {keyCount}
  </div>
  <div>
    Backspace Count: {backspaceCount}
  </div>
  <div>
    Time Taken: {timeTaken} milliseconds
  </div>
  <div>
    Average Speed: {averageSpeed.toFixed(2)} keys per second
  </div>
  <button class="calculate" on:click={validateDate}>Validate</button>
</div>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 420px;
    color: #868484;
    background: #fff;
    border-radius: 8px;
    padding: 30px 0;
    row-gap: 20px;
  }

  .date-input {
    width: auto;
    height: 60px;
    color: #444;
    font-size: 1.6rem;
    border-radius: 8px;
    border: 0.5px solid #949494;
    letter-spacing: 8px;
    padding: 10px 20px;
    box-sizing: border-box;
  }

  .error-summary {
    color: green;
  }

  .error-summary.error {
    color: red;
  }

  button.calculate {
    text-align: center;
    font-weight: 600;
    color: white;
    text-transform: uppercase;
    background-size: 200% auto;
    background-color: #dc2743;
    background-image: linear-gradient(
      to right,
      #f09433 0%,
      #e6683c 25%,
      #dc2743 50%,
      #cc2366 75%,
      #bc1888 100%
    );
    box-shadow: rgba(14, 30, 37, 0.12) 0px 2px 4px 0px,
      rgba(14, 30, 37, 0.32) 0px 2px 16px 0px;
    border: none;
    transition: 560ms;
    border-radius: 5px;
    cursor: pointer;
    padding: 20px;
    margin-top: 20px;

    &:hover {
      background-position: right center;
      -webkit-transform: scale(1.01);
      -ms-transform: scale(1.01);
      transform: scale(1.01);
    }

    &:active,
    &:focus {
      -webkit-transform: scale(1.01);
      -ms-transform: scale(1.01);
      transform: scale(1.01);
    }

    &:active {
      background-position: right center;
      -webkit-transform: scale(0.98);
      -ms-transform: scale(0.98);
      transform: scale(0.98);
    }
  }

  .notification-modal {
  display: none; /* Initially hide the notification modal */


}

.notification-content {
  text-align: center;
  padding: 0 1em;
}

.btn {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 20px;
}

.btn:hover {
  background-color: #0056b3;
}

</style>
