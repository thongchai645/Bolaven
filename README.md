<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>ลงชื่อเข้างาน - Bolaven Cafe</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
  <style>
    body { font-family: sans-serif; text-align: center; padding: 2em; background-color: #f2f2f2; }
    button, input[type="file"] { padding: 1em; font-size: 1em; border-radius: 8px; border: none; margin-top: 1em; }
    img { max-width: 80%; border-radius: 12px; margin-top: 1em; }
  </style>
</head>
<body>
  <h2>📍 ลงชื่อเข้างาน</h2>
  <p>แบรนด์: <strong>Bolaven Cafe</strong></p>
  <p id="username">กำลังโหลดชื่อผู้ใช้งาน...</p>
  <input type="file" accept="image/*" capture="environment" id="cameraInput"><br>
  <img id="preview" style="display: none;">
  <button onclick="submitData()">✅ ส่งข้อมูล</button>

  <script>
    let userDisplayName = "";
    let uploadedImage = null;

    liff.init({ liffId: "YOUR_LIFF_ID" }).then(() => {
      if (!liff.isLoggedIn()) liff.login();
      return liff.getProfile();
    }).then(profile => {
      userDisplayName = profile.displayName;
      document.getElementById("username").innerText = `👤 คุณ: ${userDisplayName}`;
    }).catch(console.error);

    const fileInput = document.getElementById("cameraInput");
    fileInput.addEventListener("change", (event) => {
      const file = event.target.files[0];
      uploadedImage = file;
      const reader = new FileReader();
      reader.onload = function(e) {
        const img = document.getElementById("preview");
        img.src = e.target.result;
        img.style.display = "block";
      }
      reader.readAsDataURL(file);
    });

    function submitData() {
      if (!uploadedImage) return alert("กรุณาถ่ายรูปก่อนส่งข้อมูล");

      const reader = new FileReader();
      reader.onload = function(e) {
        const payload = {
          name: userDisplayName,
          time: new Date().toLocaleString("th-TH", { timeZone: "Asia/Bangkok" }),
          image: e.target.result
        };

        fetch("https://script.google.com/macros/s/AKfycbx_-72gG3rAFfMU5y_l2BWDkZ7XdMNlwBrS_wbAlcvN251Y4tkpPPxMUSDNCQ4CzMJ8/exec
", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(payload)
        })
        .then(res => res.text())
        .then(msg => alert(msg))
        .catch(err => alert("เกิดข้อผิดพลาดในการส่งข้อมูล"));
      }
      reader.readAsDataURL(uploadedImage);
    }
  </script>
</body>
</html>
