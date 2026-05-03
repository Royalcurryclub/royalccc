---
id: 84
title: Contact
date: '2018-12-14T09:05:37+13:00'
author: 'RoyalCCC Reviewer'
layout: page
guid: 'http://royalccc.net/?page_id=84'
permalink: /contact/
---

Being exclusive, The Curry Club of Christchurch membership is mostly granted through referrals.

If you are a prospective member and lack a referral, please use the form below to contact our Maharaja, Rao of Christchurch GCIE KIH KBE. Make sure you include information about yourself to assist in our as of yet undefined, non-referral membership application process.

<form id="rccc-contact-form" name="contact" method="POST" action="/thanks/" data-netlify="true" netlify-honeypot="bot-field">
  <input type="hidden" name="form-name" value="contact" />
  <p style="display:none">
    <label>Do not fill this out if you are human: <input name="bot-field" /></label>
  </p>
  <p>
    <label>Your Name (required)<br>
      <input type="text" name="name" required style="width:100%; max-width:400px; padding:8px;">
    </label>
  </p>
  <p>
    <label>Your Email (required)<br>
      <input type="email" name="email" required style="width:100%; max-width:400px; padding:8px;">
    </label>
  </p>
  <p>
    <label>Subject<br>
      <input type="text" name="subject" style="width:100%; max-width:400px; padding:8px;">
    </label>
  </p>
  <p>
    <label>Your Message<br>
      <textarea name="message" rows="6" style="width:100%; max-width:400px; padding:8px;"></textarea>
    </label>
  </p>
  <p>
    <label>Which city are we based in? (required - anti-spam)<br>
      <input type="text" id="rccc-city-check" name="city-check" required style="width:100%; max-width:400px; padding:8px;">
    </label>
    <span id="rccc-city-error" style="display:none; color:#6B1E1E; font-style:italic; font-size:13px; margin-top:6px;">Sorry, that is not the city we are based in. Please try again.</span>
  </p>
  <p>
    <button type="submit" style="padding:10px 24px; background:#6B1E1E; color:#FAF4E6; border:none; cursor:pointer; font-family: -apple-system, system-ui, sans-serif; font-size: 12px; letter-spacing: 2px; text-transform: uppercase;">Send</button>
  </p>
</form>

<script>
(function () {
  var form = document.getElementById('rccc-contact-form');
  var cityField = document.getElementById('rccc-city-check');
  var errorMsg = document.getElementById('rccc-city-error');
  if (!form || !cityField || !errorMsg) return;

  form.addEventListener('submit', function (e) {
    // Normalise: trim, lowercase, strip non-letters (handles "christchurch ", "Christchurch.", "CHRISTCHURCH", "chch", etc.)
    var raw = cityField.value || '';
    var normalised = raw.toLowerCase().replace(/[^a-z]/g, '');
    var validAnswers = ['christchurch', 'chch', 'otautahi'];

    if (validAnswers.indexOf(normalised) === -1) {
      e.preventDefault();
      errorMsg.style.display = 'block';
      cityField.focus();
      cityField.style.borderColor = '#6B1E1E';
      return false;
    }
    // Otherwise let it submit normally
  });

  // Hide the error once the user starts editing again
  cityField.addEventListener('input', function () {
    errorMsg.style.display = 'none';
    cityField.style.borderColor = '';
  });
})();
</script>
