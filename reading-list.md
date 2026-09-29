---
title: Reading List
permalink: /reading-list/
---
<div class="page-head">
  <h1>Reading List</h1>
</div>

<div class="blog-intro">
  <p>Books I’ve loved, books I’ve underlined to death, books I couldn’t put down, and books I’m still thinking about long after finishing them.</p>
  <p>A running record of what I’ve read and what has managed to stay with me.</p>
</div>

<div id="reading-list-root">
  <p class="empty-note">Loading…</p>
</div>

<p style="margin-top: 3em; color: var(--ink-soft); font-size: 0.92rem;">
  This list comes from a reading-personality app I built — it reads your
  Goodreads history and figures out your taste in pace, tone, and
  character type. <a href="#" style="color: var(--navy);">Check it out</a>.
</p>
<!-- swap "#" for your deployed book recommender URL -->

<script>
(function () {
  // ==== Paste your published Google Sheet CSV link below ====
  // How to get it: File > Share > Publish to web > select the sheet > CSV > Publish
  var CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRGgn_NKnPBL27DshQfc_-nkyafvvMzLQMi8F-dCJmcditaOrXNd48chEu1vdgt9r3Ai8LWHXbzJz3q/pub?gid=0&single=true&output=csv";

  var SECTION_ORDER = ["Reading", "Loved", "Want to Read"];
  var TAG_CLASS = { "Reading": "tag-skyblue", "Loved": "tag-olive", "Want to Read": "tag-mahogany" };

  // Matches your Status column even if it's lowercase, has extra spaces, or
  // uses a slightly different phrasing. Anything else (including a BLANK
  // status) is treated as "don't show this one" - your way to leave a book
  // off the site on purpose.
  function canonicalStatus(raw) {
    var s = (raw || "").trim().toLowerCase();
    if (s === "reading" || s === "currently reading") return "Reading";
    if (s === "loved") return "Loved";
    if (s === "want to read" || s === "want-to-read" || s === "to read") return "Want to Read";
    return null;
  }

  function parseCSV(text) {
    var rows = [], row = [], field = "", inQuotes = false;
    for (var i = 0; i < text.length; i++) {
      var c = text[i], next = text[i + 1];
      if (inQuotes) {
        if (c === '"' && next === '"') { field += '"'; i++; }
        else if (c === '"') { inQuotes = false; }
        else { field += c; }
      } else {
        if (c === '"') { inQuotes = true; }
        else if (c === ',') { row.push(field); field = ""; }
        else if (c === '\n' || c === '\r') {
          if (c === '\r' && next === '\n') i++;
          row.push(field); field = "";
          if (row.length > 1 || row[0] !== "") rows.push(row);
          row = [];
        } else { field += c; }
      }
    }
    if (field !== "" || row.length) { row.push(field); rows.push(row); }
    return rows;
  }

  function escapeHTML(s) {
    return String(s).replace(/[&<>"']/g, function (c) {
      return { "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#39;" }[c];
    });
  }

  function render(rows) {
    var root = document.getElementById("reading-list-root");
    if (!rows.length) {
      root.innerHTML = '<p class="empty-note">No books yet — add a row to the sheet to get started.</p>';
      return;
    }

    var header = rows[0].map(function (h) { return h.trim().toLowerCase(); });
    var idx = {
      title: header.indexOf("title"),
      author: header.indexOf("author"),
      status: header.indexOf("status"),
      note: header.indexOf("note")
    };

    var books = rows.slice(1)
      .filter(function (r) { return r[idx.title] && r[idx.title].trim(); })
      .map(function (r) {
        return {
          title: r[idx.title] || "",
          author: r[idx.author] || "",
          status: canonicalStatus(r[idx.status]),
          note: idx.note > -1 ? (r[idx.note] || "") : ""
        };
      });

    root.innerHTML = "";
    var counter = 0;

    SECTION_ORDER.forEach(function (status) {
      var group = books.filter(function (b) { return b.status === status; });
      if (!group.length) return;

      var h2 = document.createElement("h2");
      h2.textContent = status === "Want to Read" ? "Want to read" : status;
      root.appendChild(h2);

      var list = document.createElement("div");
      list.className = "entry-list";

      group.forEach(function (b) {
        counter++;
        var row = document.createElement("div");
        row.className = "entry-row";
        row.innerHTML =
          '<span class="entry-index">' + String(counter).padStart(2, "0") + "</span>" +
          '<div class="entry-body">' +
            "<h3>" + escapeHTML(b.title) + "</h3>" +
            "<p>" + (b.author ? "by " + escapeHTML(b.author) : "") +
              (b.note ? " — " + escapeHTML(b.note) : "") + "</p>" +
            '<span class="tag ' + TAG_CLASS[status] + '">' + escapeHTML(status) + "</span>" +
          "</div>";
        list.appendChild(row);
      });

      root.appendChild(list);
    });

    if (!counter) {
      root.innerHTML = '<p class="empty-note">No books to show yet.</p>';
    }
  }

  if (CSV_URL.indexOf("PASTE_YOUR") === 0) {
    document.getElementById("reading-list-root").innerHTML =
      '<p class="empty-note">Reading list isn\u2019t connected yet \u2014 paste your published Google Sheet CSV link into reading-list.md.</p>';
    return;
  }

  fetch(CSV_URL)
    .then(function (r) { return r.text(); })
    .then(function (text) { render(parseCSV(text)); })
    .catch(function () {
      document.getElementById("reading-list-root").innerHTML =
        '<p class="empty-note">Couldn\u2019t load the reading list right now \u2014 check back soon.</p>';
    });
})();
</script>
