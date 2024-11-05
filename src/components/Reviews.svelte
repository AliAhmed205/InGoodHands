<script>
  import { onMount } from "svelte";

  let reviews = [];

  onMount(async function() {
    const addReviewButton = document.getElementById("add-review");
    const reviewFormElement = document.getElementById("review-form");
    const reviewContainerElement = document.querySelector(".adding-review");
    const reviewListElement = document.querySelector(".review-container");
    const textareaElement = document.getElementById("textarea-id");
    const characterCountElement = document.getElementById("char-count");

    const cancelReviewButton = document.getElementById("close");
    const submitReviewButton = document.getElementById("post");

    // Haal de bestaande reviews op vanuit het JSON-bestand
    try {
      const response = await fetch('https://raw.githubusercontent.com/AliAhmed205/sourceOefening/refs/heads/main/InGoodHands/babysitter.json');
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      const data = await response.json();

      // Controleer of de babysitter_service en reviews bestaan
      if (data.babysitter_service && data.babysitter_service.reviews) {
        const reviews = data.babysitter_service.reviews;
        
        // Voeg de bestaande reviews toe aan de lijst
        reviews.forEach(function(review) {
          const newReviewElement = document.createElement("li");
          newReviewElement.classList.add("review-item");

          const fullStars = '<i class="fa-solid fa-star"></i>'.repeat(review.sterren);
          const emptyStars = '<i class="fa-regular fa-star"></i>'.repeat(5 - review.sterren);

          // Gebruik de datum uit de JSON zoals deze is
          const currentDate = review.datum;

          newReviewElement.innerHTML = `
            <h3>${review.reviewer}</h3>
            <p>${fullStars}${emptyStars} <span class="review-date">${currentDate}</span></p>
            <p>${review.review}</p>
          `;

          reviewListElement.appendChild(newReviewElement);
        });
      } else {
        console.error('Geen reviews gevonden in de JSON.');
      }
    } catch (error) {
      console.error('Error fetching reviews:', error);
    }

    // Open de review-form
    addReviewButton.addEventListener("click", function() {
      reviewContainerElement.style.display = "flex";
      reviewFormElement.style.display = "block";
      setTimeout(function() {
        reviewContainerElement.classList.add("active");
        reviewFormElement.classList.add("active");
      }, 10);
    });

    // Update character count tijdens typen in textarea
    textareaElement.addEventListener("input", function() {
      const currentLength = textareaElement.value.length;
      characterCountElement.textContent = currentLength + "/135 characters used";
    });

    // Sluit de review-form met fade-out effect
    cancelReviewButton.addEventListener("click", function() {
      reviewContainerElement.classList.remove("active");
      reviewFormElement.classList.remove("active");

      setTimeout(function() {
        reviewContainerElement.style.display = "none";
        reviewFormElement.style.display = "none";
      }, 500);
    });

    // Verwerk het verzenden van een nieuwe review
    submitReviewButton.addEventListener("click", function() {
      const firstName = reviewFormElement.querySelector('input[placeholder="Aaron"]').value;
      const lastName = reviewFormElement.querySelector('input[placeholder="Berend"]').value;
      const commentary = reviewFormElement.querySelector("#textarea-id").value;
      const rating = reviewFormElement.querySelector('input[name="rating"]:checked')?.value || "No rating";

      if (firstName !== "" && lastName !== "" && commentary !== "" && rating !== "No rating") {
        // Genereer huidige datum in dd/mm/jjjj formaat
        const currentDate = new Date().toLocaleDateString("nl-NL", {
          day: "2-digit",
          month: "2-digit",
          year: "numeric"
        });

        // Maak een nieuw review-element aan
        const newReviewElement = document.createElement("li");
        newReviewElement.classList.add("review-item");

        const fullStars = '<i class="fa-solid fa-star"></i>'.repeat(rating);
        const emptyStars = '<i class="fa-regular fa-star"></i>'.repeat(5 - rating);

        newReviewElement.innerHTML = `
          <h3>${firstName} ${lastName}</h3>
          <p>${fullStars}${emptyStars}  <span class="review-date">${currentDate}</span></p>
          <p>${commentary}</p>
        `;

        newReviewElement.classList.add("appearFadeIn");

        // Voeg de nieuwe review toe aan de lijst
        reviewListElement.appendChild(newReviewElement);

        // Reset het formulier na het verzenden
        reviewFormElement.reset();

        // Sluit het formulier met fade-out effect
        reviewContainerElement.classList.remove("active");
        reviewFormElement.classList.remove("active");
        setTimeout(function() {
          reviewContainerElement.style.display = "none";
          reviewFormElement.style.display = "none";
        }, 500);
      } else {
        if (firstName === "" || lastName === "" || commentary === "") {
          alert("Please fill out all required fields.");
        } else if (rating === "No rating") {
          alert("Please provide a rating.");
        }
      }
    });
  });

  // Helper functie om sterren te genereren (voor eventueel gebruik)
  function getStars(stars) {
    return Array.from({ length: 5 }, function(value, index) {
      if (index < stars) {
        return "filled";
      } else {
        return "empty";
      }
    });
  }
</script>


<section class="adding-review">
  <form class="review-form" id="review-form" action="" method="post">
    <p>
      <b>Hired on of our babysitters before?</b> <br />Tell us about your
      experience!
    </p>
    <label for="first-name"
      >First name
      <input type="text" placeholder="Aaron" />
    </label>
    <label for="last-name"
      >Last name
      <input type="text" placeholder="Berend" />
    </label>
    <label for="e-mail"
      >E-mail
      <input
        type="email"
        name="email-adress"
        id="email-id"
        placeholder="yourmail@mail.com"
      />
    </label>
    <label for="text-area-label" id="area-label-id">
      Commentary
      <textarea maxlength="135" name="textarea-name" id="textarea-id"
      ></textarea>
      <p id="char-count">0/135 characters used</p>
    </label>
    <label class="rating-label" for="rating"
      >How many stars would you rate us?</label
    >
    <div class="star-rating">
      <input type="radio" name="rating" id="star5" value="5" />
      <label for="star5" title="5 stars"><i class="fa-solid fa-star"></i></label
      >
      <input type="radio" name="rating" id="star4" value="4" />
      <label for="star4" title="4 stars"><i class="fa-solid fa-star"></i></label
      >
      <input type="radio" name="rating" id="star3" value="3" />
      <label for="star3" title="3 stars"><i class="fa-solid fa-star"></i></label
      >
      <input type="radio" name="rating" id="star2" value="2" />
      <label for="star2" title="2 stars"><i class="fa-solid fa-star"></i></label
      >
      <input type="radio" name="rating" id="star1" value="1" />
      <label for="star1" title="1 star"><i class="fa-solid fa-star"></i></label>
    </div>
    <div class="button-container">
      <button type="button" id="post">submit review</button>
      <button type="button" id="close">cancel</button>
    </div>
  </form>
</section>
<section id="review-anchor" class="review-parent">
  <h2 class="reviews-title">Reviews</h2>
  <ul class="review-container">
    <button type="button" id="add-review"><span>+</span> Add Review</button>
  </ul>
</section>
