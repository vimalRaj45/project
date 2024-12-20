<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>OTP Verification</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
  <script type="text/javascript">
    (function(){
      emailjs.init("W_iMgtG4kUORkD7in"); // Initialize EmailJS with your public key
    })();
  </script>
  <style>
    .container {
      margin-top: 10%;
      max-width: 400px;
    }
    input {
      margin-bottom: 15px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h3 class="text-center">OTP Verification</h3>
    <form id="otpForm" onsubmit="return handleSubmit(event)">
      <div class="mb-3">
        <label for="email" class="form-label">Email Address</label>
        <input type="email" id="email" name="email" class="form-control" placeholder="Enter your email" required>
      </div>
      <div class="mb-3 d-none" id="otp-container">
        <label for="otp" class="form-label">Enter OTP</label>
        <input type="text" id="otp" name="otp" class="form-control" placeholder="Enter the OTP" maxlength="6">
      </div>
      <div class="d-grid gap-2">
        <button type="submit" class="btn btn-primary" id="send-otp">Send OTP</button>
        <button type="button" class="btn btn-success d-none" id="verify-otp" onclick="verifyOTP()">Verify OTP</button>
      </div>
    </form>
  </div>

  <script>
    let generatedOTP;

    function handleSubmit(event) {
      event.preventDefault(); // Prevent form submission

      const emailInput = document.getElementById("email").value.trim();
      const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

      if (!emailPattern.test(emailInput)) {
        alert("Please enter a valid email address.");
        return;
      }

      // Generate OTP
      generatedOTP = Math.floor(100000 + Math.random() * 900000);

      // Send OTP via EmailJS
      const params = {
        email: emailInput,
        otp: generatedOTP,
        from_name: "Sethu properties", // Adjust as needed
        to_name: emailInput,
        message: `Your OTP is ${generatedOTP}. Please use it to verify your email.`,
        reply_to: "noreply@yourwebsite.com" 
      };

      emailjs.send("service_gs473zw", "template_6kvn4cd", params)
        .then(function(response) {
          alert("OTP sent successfully! Please check your email.");

          // Update UI
          document.getElementById("otp-container").classList.remove("d-none");
          document.getElementById("verify-otp").classList.remove("d-none");
          document.getElementById("send-otp").textContent = "Resend OTP";
        })
        .catch(function(error) {
          console.error("Failed to send email:", error);
          alert("Failed to send OTP. Please try again.");
        });
    }

    function verifyOTP() {
      const userOTP = document.getElementById("otp").value.trim();

      if (!userOTP) {
        alert("Please enter the OTP.");
        return;
      }

      if (userOTP === String(generatedOTP)) {
        alert("OTP verified successfully!");
      } else {
        alert("Invalid OTP. Please try again.");
      }
    }
  </script>
</body>
</html>
