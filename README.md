<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<title>筆記索引圖書館</title>
<style>
body { font-family: Arial, sans-serif; padding: 20px; background-color: #f9f9f9; }
h1 { color: #2a7ae2; }
h2 { cursor: pointer; color: #2a7ae2; }
ul { list-style: none; padding-left: 20px; display: none; }
li { margin: 5px 0; }
input { padding: 5px; width: 300px; margin-bottom: 20px; }
a { text-decoration: none; color: #1a73e8; }
</style>
</head>
<body>

<h1>我的筆記索引圖書館</h1>
<input type="text" id="searchBox" placeholder="搜尋筆記標題或代碼...">

<div id="library"></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/tabletop.js/1.5.1/tabletop.min.js"></script>
<script>
const SHEET_URL = '你的 Google Sheet 分享連結';

// 讀取 Google Sheet
Tabletop.init({
  key: SHEET_URL,
  callback: showNotes,
  simpleSheet: true
});

function showNotes(data) {
  const libraryDiv = document.getElementById("library");
  const subjects = {};

  data.forEach(row => {
    if (!subjects[row.SubjectCode]) subjects[row.SubjectCode] = {name: row.SubjectName, notes: []};
    subjects[row.SubjectCode].notes.push(row);
  });

  for (let code in subjects) {
    let h2 = document.createElement('h2');
    h2.textContent = `${code} ${subjects[code].name}`;
    let ul = document.createElement('ul');
    ul.style.display = 'none';
    h2.onclick = () => { ul.style.display = ul.style.display === 'none' ? 'block' : 'none'; };

    subjects[code].notes.forEach(note => {
      let li = document.createElement('li');
      li.innerHTML = `<b>${note.NoteID}</b>：${note.NoteTitle} [${note.Category}] 
        <a href="${note.DriveLink}" target="_blank">開啟筆記</a>`;
      ul.appendChild(li);
    });

    libraryDiv.appendChild(h2);
    libraryDiv.appendChild(ul);
  }
}

// 搜尋功能
document.getElementById("searchBox").addEventListener("input", function() {
  let query = this.value.toLowerCase();
  let lists = document.querySelectorAll("#library ul li");
  lists.forEach(li => {
    li.style.display = li.textContent.toLowerCase().includes(query) ? "block" : "none";
  });
});
</script>

</body>
</html>
# ZE-
