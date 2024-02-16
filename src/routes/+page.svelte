<div class="text-center mt-5">
  <h1>Select Your Birthdate</h1>

  <ul>
    <li>Dear Participant, the purpose of conducting these tests is to evaluate the functionality of the user interface (UI) rather than assessing your individual performance.</li>
    <li>We encourage you to explore and feel comfortable making mistakes; our primary focus is on examining the UI's functionality.</li>
    <li>Our main objective is to assess the efficiency and accuracy of date entry.</li>
    <li>Please ensure to focus on selecting your birth date accurately, giving priority to accuracy over speed.</li>
    <li>Click on the "Next" button below to proceed. Thank you.</li>
  </ul>

  <button class="btn btn-primary mt-3" onclick="redirectToVariantA()">Next</button>

  <script>
    function redirectToVariantA() {
      window.location.href = '/variant-a';
    }
  </script>


 <style>
    .text-center {
      display: flex;
      flex-direction: column;
      align-items: center;
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
  </style>
</div>
