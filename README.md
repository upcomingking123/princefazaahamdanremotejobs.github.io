# princefazaahamdanremotejobs.github.io
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>fazaahamdanremotejobs.com</title>
  <script src="https://js.stripe.com/v3/"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
    
    body {
      font-family: 'Inter', sans-serif;
      margin: 0;
      background: #f8fafc;
      color: #1f2937;
    }
    .header {
      background: linear-gradient(135deg, #1e40af, #3b82f6);
      color: white;
      padding: 25px 20px;
      text-align: center;
    }
    .header h1 {
      margin: 0;
      font-size: 26px;
      font-weight: 700;
    }
    .container {
      max-width: 600px;
      margin: 20px auto;
      padding: 20px;
      background: white;
      border-radius: 20px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.1);
    }
    h2 {
      font-size: 22px;
      margin-bottom: 20px;
      color: #1e40af;
    }
    input, select, textarea, button {
      width: 100%;
      padding: 15px;
      margin: 10px 0;
      border: 1px solid #e5e7eb;
      border-radius: 12px;
      font-size: 16px;
    }
    button {
      background: #2563eb;
      color: white;
      font-weight: 600;
      cursor: pointer;
      border: none;
    }
    button:active {
      background: #1e40af;
    }
    .step {
      display: none;
    }
    .step.active {
      display: block;
    }
    .progress {
      display: flex;
      justify-content: space-between;
      margin-bottom: 30px;
    }
    .progress div {
      width: 32px;
      height: 32px;
      border-radius: 50%;
      background: #e5e7eb;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: bold;
    }
    .progress div.active {
      background: #2563eb;
      color: white;
    }
    .upload-box {
      border: 2px dashed #94a3b8;
      padding: 20px;
      text-align: center;
      border-radius: 12px;
      margin: 10px 0;
      background: #f8fafc;
    }
    .plan-option {
      border: 2px solid #e5e7eb;
      padding: 15px;
      margin: 10px 0;
      border-radius: 12px;
      cursor: pointer;
      transition: all 0.3s;
    }
    .plan-option.selected {
      border-color: #2563eb;
      background: #f0f9ff;
    }
    .price {
      font-size: 24px;
      font-weight: 700;
      color: #1e40af;
    }
    .stripe-card {
      border: 1px solid #e5e7eb;
      padding: 15px;
      border-radius: 12px;
      background: #fafafa;
      min-height: 50px;
    }
    .form-row {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
    }
    .note {
      font-size: 14px;
      color: #64748b;
      margin: 10px 0;
    }
  </style>
</head>
<body>
  <div class="header">
    <h1>info.remotejobs.com</h1>
  </div>

  <div class="container">
    <div class="progress" id="progress">
      <div class="active">1</div>
      <div>2</div>
      <div>3</div>
      <div>4</div>
      <div>5</div>
    </div>

    <!-- Step 1: Account Creation -->
    <div class="step active" id="step1">
      <h2>Create Your Account</h2>
      <input type="text" id="fullName" placeholder="Full Name" required>
      <input type="email" id="email" placeholder="Email Address" required>
      <input type="password" id="password" placeholder="Create Password" required>
      <button onclick="nextStep(2)">Continue</button>
    </div>

    <!-- Step 2: Job Categories -->
    <div class="step" id="step2">
      <h2>Select Jobs You Are Familiar With</h2>
      <div id="categoriesContainer" style="margin-bottom: 20px;"></div>
      <button onclick="nextStep(3)">Next</button>
    </div>

    <!-- Step 3: Qualifications -->
    <div class="step" id="step3">
      <h2>Your Qualifications</h2>
      <textarea id="experience" rows="4" placeholder="Describe your experience..."></textarea>
      <input type="text" id="skills" placeholder="Skills (comma separated)">
      <button onclick="nextStep(4)">Next</button>
    </div>

    <!-- Step 4: ID Upload -->
    <div class="step" id="step4">
      <h2>Upload Your ID Card (Front & Back)</h2>
      <div class="upload-box">
        <p><strong>ID Front Side</strong></p>
        <input type="file" accept="image/*" id="idFront">
      </div>
      <div class="upload-box">
        <p><strong>ID Back Side</strong></p>
        <input type="file" accept="image/*" id="idBack">
      </div>
      <button onclick="nextStep(5)">Next</button>
    </div>

    <!-- Step 5: Subscription & Payment -->
    <div class="step" id="step5">
      <h2>Choose Your Plan</h2>
      
      <div id="planOptions">
        <!-- Populated by JS -->
      </div>

      <!-- Premium Payment Section (shown only for premium) -->
      <div id="premiumPayment" style="display: none; margin-top: 25px;">
        <h3>Payment Details</h3>
        <p class="note">Subscription fee will be charged via Stripe. $20/month or $190/year.</p>
        
        <div class="form-row">
          <input type="text" id="cardName" placeholder="Full Name on Card" required>
          <input type="email" id="paymentEmail" placeholder="Billing Email" value="" required>
        </div>

        <div id="card-element" class="stripe-card">
          <!-- Stripe Card Element will be mounted here -->
        </div>
        <div id="card-errors" style="color: #ef4444; font-size: 14px; margin-top: 8px;"></div>

        <div class="form-row">
          <input type="text" id="address" placeholder="Address" required>
          <input type="text" id="suburb" placeholder="Suburb">
        </div>
        <div class="form-row">
          <input type="text" id="city" placeholder="City" required>
          <input type="text" id="state" placeholder="State" required>
        </div>
        <input type="text" id="postalCode" placeholder="Postal Code" required>

        <div style="margin: 20px 0;">
          <label>
            <input type="checkbox" id="tos" required> I agree to the <a href="#" style="color:#2563eb;">Terms of Service</a> and <a href="#" style="color:#2563eb;">Privacy Policy</a>
          </label>
        </div>
      </div>

      <!-- Withdrawal Info (for all users) -->
      <h3 style="margin-top: 30px;">Bank Card for Withdrawal</h3>
      <div class="upload-box">
        <p><strong>Bank Card Front</strong></p>
        <input type="file" accept="image/*" id="bankFront">
      </div>
      <div class="upload-box">
        <p><strong>Bank Card Back</strong></p>
        <input type="file" accept="image/*" id="bankBack">
      </div>
      <select id="paymentMethod">
        <option value="">Select Withdrawal Method</option>
        <option value="bank">Bank Transfer</option>
        <option value="paypal">PayPal</option>
      </select>
      <input type="text" id="bankName" placeholder="Preferred Bank Name">

      <button onclick="completeSetup()" style="margin-top: 20px;">Complete Setup & Pay (if premium)</button>
    </div>
  </div>

  <script>
    let currentStep = 1;
    let selectedPlan = 'free';
    let stripe;
    let card;

    const categories = ['Software Development', 'Customer Service', 'Writing & Content', 'Graphic Design', 'Digital Marketing', 'Data Entry', 'Virtual Assistance', 'Sales'];

    // Populate categories
    const container = document.getElementById('categoriesContainer');
    categories.forEach(cat => {
      const div = document.createElement('div');
      div.style.margin = '8px 0';
      div.innerHTML = `<label style="font-size:16px"><input type="checkbox" value="${cat}"> ${cat}</label>`;
      container.appendChild(div);
    });

    // Populate Plan Options
    function populatePlans() {
      const planContainer = document.getElementById('planOptions');
      planContainer.innerHTML = `
        <div class="plan-option selected" id="plan-free" onclick="selectPlan('free')">
          <strong>Free Plan</strong><br>
          <span style="font-size:14px; color:#64748b;">Access to available jobs (limited)</span><br>
          <span class="price">$0</span>
        </div>
        
        <div class="plan-option" id="plan-premium" onclick="selectPlan('premium')">
          <strong>Premium Plan</strong><br>
          <span style="font-size:14px; color:#64748b;">Access to 100+ jobs • Priority support</span><br>
          <span class="price">$20<span style="font-size:14px; font-weight:400;">/month</span></span><br>
          <small>or $190/year (save ~21%)</small>
        </div>
      `;
    }

    function selectPlan(plan) {
      selectedPlan = plan;
      
      document.querySelectorAll('.plan-option').forEach(el => {
        el.classList.remove('selected');
      });
      
      if (plan === 'free') {
        document.getElementById('plan-free').classList.add('selected');
        document.getElementById('premiumPayment').style.display = 'none';
      } else {
        document.getElementById('plan-premium').classList.add('selected');
        document.getElementById('premiumPayment').style.display = 'block';
        
        // Pre-fill email
        const userEmail = document.getElementById('email').value;
        if (userEmail) document.getElementById('paymentEmail').value = userEmail;
      }
    }

    function nextStep(step) {
      document.getElementById(`step${currentStep}`).classList.remove('active');
      document.getElementById(`step${step}`).classList.add('active');
      currentStep = step;
      updateProgress();
      
      if (step === 5) {
        populatePlans();
        initStripe();
      }
    }

    function updateProgress() {
      const progress = document.getElementById('progress');
      for (let i = 0; i < 5; i++) {
        progress.children[i].classList.toggle('active', i < currentStep);
      }
    }

    // Initialize Stripe
    function initStripe() {
      if (typeof Stripe === 'undefined') {
        console.warn("Stripe script not loaded");
        return;
      }
      
      // Replace with your real Publishable Key in production
      stripe = Stripe('pk_test_YOUR_TEST_KEY_HERE'); 
      
      if (!card) {
        const elements = stripe.elements();
        card = elements.create('card', {
          style: {
            base: {
              fontSize: '16px',
              color: '#1f2937',
            }
          }
        });
        card.mount('#card-element');
      }
    }

    async function completeSetup() {
      const name = document.getElementById('fullName').value || "User";
      const isPremium = selectedPlan === 'premium';

      if (isPremium) {
        // Simulate payment processing (replace with real Stripe confirmCardPayment in backend integration)
        const cardName = document.getElementById('cardName').value;
        const address = document.getElementById('address').value;
        
        if (!cardName || !address) {
          alert("Please fill in all billing information.");
          return;
        }

        // Mock payment success
        alert(`✅ Payment Successful!\n\nWelcome ${name}!\n\nYou are now a Premium member.\nYou have access to 100+ remote jobs.\n\nThank you for subscribing!`);
      } else {
        alert(`✅ Success!\n\nWelcome ${name}!\n\nYour Free account on info.remotejobs.com is ready.\nYou can now browse available remote jobs.`);
      }
      
      // In a real app, you would send all data to your backend here
      console.log({
        plan: selectedPlan,
        name,
        email: document.getElementById('email').value,
        // ... other data
      });
    }

    // Initialize
    updateProgress();
  </script>
</body>
</html>