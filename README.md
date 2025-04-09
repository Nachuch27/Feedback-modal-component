<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Feedback Modal</title>
  <style>
    /* 
      Dark overlay to dim the background when modal is open 
    */
    .modal-overlay {
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: rgba(0, 0, 0, 0.5); /* semi-transparent black */
      display: none; /* hidden by default */
      align-items: center;
      justify-content: center;
      z-index: 999; /* sit above everything else */
    }

    /*
      Main modal box styling
    */
    .modal {
      background: white;
      padding: 2rem;
      border-radius: 10px;
      max-width: 300px;
      width: 90%;
      text-align: center;
      position: relative;
    }

    /*
      Container for rating buttons (1-5)
    */
    .ratings {
      display: flex;
      justify-content: space-between;
      margin: 1rem 0;
    }

    /*
      Each rating button: circular and clean
    */
    .ratings button {
      width: 40px;
      height: 40px;
      border: 2px solid #ddd;
      border-radius: 50%;
      background: none;
      cursor: pointer;
      transition: 0.2s ease;
    }

    /* 
      Highlight the selected rating button
    */
    .ratings button.selected {
      background-color: #007bff;
      color: white;
      border-color: #007bff;
    }

    /* 
      General button style
    */
    .btn {
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    /* 
      Primary button (green style)
    */
    .btn-primary {
      background-color: #28a745;
      color: white;
    }

    /*
      Close (X) button for the modal
    */
    .btn-close {
      position: absolute;
      top: 10px; right: 15px;
      background: none;
      font-size: 1.2rem;
      border: none;
      cursor: pointer;
    }

    /*
      Thank-you message (initially hidden)
    */
    .thank-you {
      display: none;
      color: green;
      font-weight: bold;
      margin-top: 1rem;
    }
  </style>
</head>
<body>

  <!-- Button to open the modal -->
  <button onclick="openModal()" class="btn btn-primary">Give Feedback</button>

  <!-- Feedback modal -->
  <div class="modal-overlay" id="modalOverlay">
    <div class="modal">
      <!-- Close button inside the modal -->
      <button class="btn-close" onclick="closeModal()">&times;</button>

      <h3>How was your experience?</h3>

      <!-- Container for rating buttons -->
      <div class="ratings" id="ratingButtons">
        <!-- Rating buttons will be generated using JavaScript -->
      </div>

      <!-- Submit feedback -->
      <button class="btn btn-primary" onclick="submitFeedback()">Submit</button>

      <!-- Thank-you message shown after submission -->
      <p class="thank-you" id="thankYouMessage">Thanks for your feedback!</p>
    </div>
  </div>

  <script>
    // Reference to modal and thank-you elements
    const modalOverlay = document.getElementById('modalOverlay');
    const ratingButtonsContainer = document.getElementById('ratingButtons');
    const thankYouMessage = document.getElementById('thankYouMessage');

    // Store which rating was clicked (1-5)
    let selectedRating = null;

    // Show the modal
    function openModal() {
      modalOverlay.style.display = 'flex'; // reveal the modal
      thankYouMessage.style.display = 'none'; // reset the thank-you message
      selectedRating = null; // clear previous selection
      renderRatings(); // regenerate buttons
    }

    // Hide the modal
    function closeModal() {
      modalOverlay.style.display = 'none';
    }

    // Create and display the 1-5 rating buttons
    function renderRatings() {
      ratingButtonsContainer.innerHTML = ''; // clear old buttons
      for (let i = 1; i <= 5; i++) {
        const btn = document.createElement('button');
        btn.textContent = i;
        btn.onclick = () => selectRating(i); // handle click
        if (selectedRating === i) {
          btn.classList.add('selected'); // highlight selected
        }
        ratingButtonsContainer.appendChild(btn);
      }
    }

    // Handle rating selection
    function selectRating(value) {
      selectedRating = value;
      renderRatings(); // refresh buttons to show highlight
    }

    // Save the selected rating to localStorage and show a thank-you
    function submitFeedback() {
      if (!selectedRating) {
        alert("Please select a rating first!");
        return;
      }

      // Get any previous ratings from localStorage (or start a new list)
      let ratings = JSON.parse(localStorage.getItem('feedbackRatings')) || [];

      // Add the new rating
      ratings.push(selectedRating);

      // Save it back to localStorage
      localStorage.setItem('feedbackRatings', JSON.stringify(ratings));

      // Show thank-you message
      thankYouMessage.style.display = 'block';
    }

    // When page loads, set up rating buttons
    renderRatings();
  </script>

</body>
</html>