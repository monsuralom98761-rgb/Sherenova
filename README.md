
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ShareNova | Share & Earn</title>
<style>
* { box-sizing: border-box; }

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f4f5fb;
  color: #1d2340;
}

header {
  padding: 18px;
  background: #241d57;
  color: white;
  font-size: 24px;
  font-weight: bold;
}

header span { color: #b7aaff; }

.container {
  max-width: 650px;
  margin: 24px auto;
  padding: 16px;
}

.card {
  background: white;
  padding: 22px;
  margin-bottom: 18px;
  border-radius: 16px;
  box-shadow: 0 5px 20px #0000000b;
}

.balance {
  color: white;
  background: linear-gradient(120deg, #30236f, #7659ed);
}

.balance h1 {
  font-size: 38px;
  margin: 10px 0;
}

.muted { color: #727891; line-height: 1.5; }
.balance .muted { color: #e4deff; }

input {
  width: 100%;
  padding: 13px;
  margin: 8px 0;
  border: 1px solid #d9dced;
  border-radius: 9px;
  font-size: 15px;
}

button {
  width: 100%;
  border: 0;
  padding: 13px;
  margin-top: 10px;
  border-radius: 9px;
  font-weight: bold;
  font-size: 15px;
  cursor: pointer;
}

.primary { background: #6752df; color: white; }
.secondary { background: #eeeaff; color: #4937bb; }
.disabled { background: #e7e8ef; color: #656b7c; }

.notice {
  background: #fff6df;
  color: #77520c;
  padding: 12px;
  border-radius: 9px;
  line-height: 1.5;
}

#message {
  min-height: 20px;
  font-size: 14px;
  line-height: 1.5;
}

footer {
  text-align: center;
  color: #777d91;
  padding: 25px;
  font-size: 12px;
}
</style>
</head>

<body>
<header>Share<span>Nova</span></header>

<main class="container">

  <section class="card">
    <h2>Welcome to ShareNova</h2>
    <p class="muted">
      Share your referral link and track eligible referral rewards.
    </p>
    <label for="username">Your display name</label>
    <input id="username" placeholder="Enter your display name">
    <button class="primary" onclick="openDashboard()">
      Open Dashboard
    </button>
  </section>

  <section id="dashboard" class="hidden">
    <div class="card balance">
      <p>Reward Balance (Prototype)</p>
      <h1 id="balance">$0.00</h1>
      <p class="muted">
        Referral rewards remain pending until verified and approved.
        This prototype does not hold real money.
      </p>
    </div>

    <div class="card">
      <h2>Share & Earn</h2>
      <p class="muted">
        Reward rule: $5 for an eligible new signup through your referral link,
        subject to verification and the published reward terms.
      </p>

      <input id="referral" readonly>
      <button class="secondary" onclick="copyReferral()">
        Copy Referral Link
      </button>

      <p id="message" role="status"></p>
    </div>

    <div class="card">
      <h2>Referral Reward</h2>
      <p class="muted">
        A new signup does not instantly create withdrawable money.
        Rewards require server-side checks and approval.
      </p>
      <button class="primary" onclick="recordDemoReferral()">
        Preview $5 Pending Reward
      </button>
    </div>

    <div class="card">
      <h2>Withdraw</h2>
      <p>Minimum withdrawal: $100 equivalent.</p>
      <div class="notice">
        Withdrawals are not enabled yet. No deposit is required in this
        prototype. Payment availability and eligibility must be confirmed
        before a real withdrawal service launches.
      </div>
      <button class="disabled" onclick="showWithdrawalInfo()">
        Check Withdrawal Information
      </button>
    </div>

    <div class="card">
      <h2>Activity</h2>
      <div id="history" class="muted">No activity yet.</div>
    </div>
  </section>
</main>

<footer>
  ShareNova · Prototype only · No real deposits or payouts
</footer>

<script>
let pendingReward = 0;
let demoReferralCount = 0;

function openDashboard() {
  const name = document.getElementById("username").value.trim();

  if (!name) {
    alert("Please enter your display name.");
    return;
  }

  document.getElementById("dashboard").classList.remove("hidden");

  // Local preview only: not a verified identity or real account.
  const code = "DEMO" + Math.random().toString(36).slice(2, 8).toUpperCase();
  document.getElementById("referral").value =
    window.location.href.split("?")[0] + "?ref=" + code;
}

async function copyReferral() {
  const input = document.getElementById("referral");

  try {
    await navigator.clipboard.writeText(input.value);
    document.getElementById("message").textContent =
      "Demo referral link copied.";
  } catch {
    input.select();
    document.getElementById("message").textContent =
      "Select and copy the link manually.";
  }
}

function recordDemoReferral() {
  // Explicitly a preview: no real user signup is verified here.
  pendingReward += 5;
  demoReferralCount++;

  document.getElementById("balance").textContent =
    "$" + pendingReward.toFixed(2);

  const history = document.getElementById("history");
  history.innerHTML = "";

  const item = document.createElement("p");
  item.textContent =
    "Demo referral #" + demoReferralCount +
    " — $5.00 — Pending Review (not real money)";
  history.appendChild(item);

  document.getElementById("message").textContent =
    "Demo reward preview added. No real balance was credited.";
}

function showWithdrawalInfo() {
  alert(
    "Withdrawals are not available in this prototype. " +
    "No deposit has been taken. Real payouts require a verified reward " +
    "ledger, an approved payment provider, and compliance review."
  );
}
</script>
</body>
</html>
