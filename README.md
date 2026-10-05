<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Sushi Zanmai Manager Clock-In</title>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f6f8;
      text-align: center;
      margin: 0;
      padding: 30px 15px;
      color: #333;
    }

    h1 {
      font-size: 26px;
      margin-bottom: 15px;
    }

    #message {
      margin: 20px auto;
      max-width: 700px;
      font-size: 18px;
      line-height: 1.6;
    }

    iframe {
      width: 100%;
      height: 85vh;
      min-height: 700px;
      border: none;
      margin-top: 15px;
      background: white;
    }
  </style>
</head>

<body>

  <h1>Sushi Zanmai Manager Clock-In</h1>

  <div id="message">
    Please scan your Manager QR code to begin.
  </div>

  <iframe
    id="jotformFrame"
    src=""
    style="display: none;"
    allow="geolocation; clipboard-read; clipboard-write; camera; microphone">
  </iframe>


  <script>

    // ==================================================
    // JOTFORM
    // ==================================================

    const jotformBase =
      "https://form.jotform.com/262753378205461";


    // ==================================================
    // READ URL PARAMETERS
    // ==================================================

    const urlParams =
      new URLSearchParams(window.location.search);

    const managerFromURL =
      urlParams.get("manager");

    const outletFromURL =
      urlParams.get("outlet");


    // ==================================================
    // GET SAVED MANAGER NAME
    // ==================================================

    let savedName =
      localStorage.getItem("managerName");


    // ==================================================
    // MANAGER QR CODE
    //
    // Example:
    // ?manager=LOH%20MUI%20SENG
    // ==================================================

    if (managerFromURL) {

      savedName = managerFromURL;

      localStorage.setItem(
        "managerName",
        savedName
      );

      document.getElementById("message").innerHTML =
        "✅ Manager name saved successfully.<br><br>" +
        "Please continue to use this browser for future clock-ins " +
        "by tapping your outlet's NFC tag.";
    }


    // ==================================================
    // OUTLET NFC TAG
    //
    // Example:
    // ?outlet=KURA
    // ==================================================

    if (outletFromURL) {

      // ------------------------------------------------
      // MANAGER NAME NOT FOUND
      // ------------------------------------------------

      if (!savedName) {

        document.getElementById("message").innerHTML =
          "⚠️ Manager name not found.<br><br>" +
          "Please save your name first before using " +
          "the outlet NFC tag for clock-in.";

      }


      // ------------------------------------------------
      // MANAGER NAME FOUND
      // ------------------------------------------------

      else {

        const outlet =
          outletFromURL;

        // ----------------------------------------------
        // BUILD JOTFORM PREFILL URL
        // ----------------------------------------------

        const jotformURL =
          jotformBase +
          "?name=" +
          encodeURIComponent(savedName) +
          "&outlet=" +
          encodeURIComponent(outlet);


        // ----------------------------------------------
        // LOAD JOTFORM
        // ----------------------------------------------

        const iframe =
          document.getElementById("jotformFrame");

        iframe.src = jotformURL;

        iframe.style.display = "block";


        // ----------------------------------------------
        // MESSAGE
        // ----------------------------------------------

        document.getElementById("message").innerHTML =
          "✅ Manager: <strong>" +
          escapeHTML(savedName) +
          "</strong><br>" +
          "📍 Outlet: <strong>" +
          escapeHTML(outlet) +
          "</strong><br><br>" +
          "Please complete your clock-in.";
      }
    }


    // ==================================================
    // CLEAN THE ADDRESS BAR
    //
    // Removes:
    //
    // ?manager=...
    // ?outlet=...
    //
    // from:
    //
    // https://tomloh-sz.github.io/sushizanmai/
    // ==================================================

    if (managerFromURL || outletFromURL) {

      window.history.replaceState(
        {},
        document.title,
        window.location.pathname
      );

    }


    // ==================================================
    // SIMPLE HTML ESCAPE
    // ==================================================

    function escapeHTML(text) {

      return String(text)
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#039;");

    }

  </script>

</body>
</html>
