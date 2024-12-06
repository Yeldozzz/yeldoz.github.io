<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Anime List</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            padding: 20px;
        }

        h1 {
            text-align: center;
            color: #333;
        }

        button {
            padding: 10px 20px;
            margin: 10px 5px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
            font-size: 16px;
            border-radius: 5px;
        }

        button:hover {
            background-color: #45a049;
        }

        table {
            width: 100%;
            margin: 20px 0;
            border-collapse: collapse;
            background-color: white;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        }

        th, td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }

        th {
            background-color: #4CAF50;
            color: white;
            text-transform: uppercase;
        }

        tr:hover {
            background-color: #f1f1f1;
        }

        td {
            color: #555;
        }

        td:nth-child(4) {
            text-align: center;
        }

        .button-container {
            text-align: center;
        }

        ul {
            margin: 20px 0;
            padding: 0;
            list-style-type: disc;
            color: #555;
        }

        ul li {
            margin: 5px 0;
            padding: 5px;
            background-color: #fff;
            border-left: 4px solid #4CAF50;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        }
    </style>
</head>
<body>
    <h1>Anime List</h1>

    <div class="button-container">
        <button onclick="sortTable()">Sort by Year</button>
        <button onclick="filterTable()">Filter by Rating > 8.5</button>
    </div>

    <table id="animeTable">
        <thead>
            <tr>
                <th>Title</th>
                <th>Genre</th>
                <th>Year</th>
                <th>Rating</th>
            </tr>
        </thead>
        <tbody>
   
        </tbody>
    </table>

    <h2>Список аниме листа</h2>
    <ul id="animeList">
   
    </ul>

    <script>
        function loadXML() {
            const xhr = new XMLHttpRequest();
            xhr.open("GET", "anime.xml", true);
            xhr.onreadystatechange = function() {
                if (xhr.readyState === 4 && xhr.status === 200) {
                    const xml = xhr.responseXML;
                    displayData(xml);
                    populateList(xml);
                }
            };
            xhr.send();
        }

        function displayData(xml) {
            const table = document.getElementById('animeTable').getElementsByTagName('tbody')[0];
            const entries = xml.getElementsByTagName('entry');
            
            for (let i = 0; i < entries.length; i++) {
                const entry = entries[i];
                const title = entry.getElementsByTagName('title')[0].textContent;
                const genre = entry.getElementsByTagName('genre')[0].textContent;
                const year = entry.getElementsByTagName('year')[0].textContent;
                const rating = entry.getElementsByTagName('rating')[0].textContent;

                const row = table.insertRow();
                row.insertCell(0).textContent = title;
                row.insertCell(1).textContent = genre;
                row.insertCell(2).textContent = year;
                row.insertCell(3).textContent = rating;
            }
        }

        function populateList(xml) {
            const list = document.getElementById('animeList');
            const entries = xml.getElementsByTagName('entry');

            for (let i = 0; i < entries.length; i++) {
                const title = entries[i].getElementsByTagName('title')[0].textContent;
                const listItem = document.createElement('li');
                listItem.textContent = title;
                list.appendChild(listItem);
            }
        }

        function sortTable() {
            const table = document.getElementById('animeTable');
            const rows = Array.from(table.rows).slice(1);
            rows.sort((rowA, rowB) => {
                const yearA = parseInt(rowA.cells[2].textContent);
                const yearB = parseInt(rowB.cells[2].textContent);
                return yearA - yearB;
            });
            rows.forEach(row => table.appendChild(row));
        }

        function filterTable() {
            const table = document.getElementById('animeTable');
            const rows = Array.from(table.rows).slice(1);
            rows.forEach(row => {
                const rating = parseFloat(row.cells[3].textContent);
                if (rating <= 8.5) {
                    row.style.display = 'none';
                } else {
                    row.style.display = '';
                }
            });
        }

        window.onload = loadXML;
    </script>
</body>
</html>
