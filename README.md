<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Аниме и Манга</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        h1 {
            text-align: center;
        }
        button {
            margin: 5px;
            padding: 8px 12px;
            cursor: pointer;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid black;
            padding: 8px;
            text-align: center;
        }
        th {
            background-color: #f4f4f4;
        }
        #dialog {
            display: none;
            border: 2px solid black;
            padding: 10px;
            background-color: white;
            position: absolute;
            top: 20%;
            left: 30%;
            width: 40%;
        }
        #dialog p {
            margin: 10px 0;
        }
    </style>
</head>
<body>
    <div id="animeApp">
        <h1>Список Аниме и Манги</h1>

        <div>
            <button onclick="addAnimeWithPrompt()">Добавить новое аниме</button>
            <button onclick="removeAnime()">Удалить последнее аниме</button>
            <button onclick="restoreAnime()">Восстановить последнее удаленное</button>
            <button onclick="toggleTable()">Показать/Скрыть таблицу</button>
            <button onclick="openDialog()">Показать информацию</button>
        </div>

        <table id="animeTable">
            <thead>
                <tr>
                    <th>№</th>
                    <th>Название</th>
                    <th>Жанр</th>
                    <th>Рейтинг</th>
                </tr>
            </thead>
            <tbody>
                <!-- Таблица заполняется через JavaScript -->
            </tbody>
        </table>
        <p id="emptyMessage" style="text-align: center; display: none;">Список пуст</p>

        <div id="dialog">
            <p><strong>Название:</strong> <span id="dialogTitle"></span></p>
            <p><strong>Жанр:</strong> <span id="dialogGenre"></span></p>
            <p><strong>Рейтинг:</strong> <span id="dialogRating"></span></p>
            <button onclick="closeDialog()">Закрыть</button>
        </div>
    </div>

    <script>
        const animeList = [
            { title: 'Naruto', genre: 'Shounen', rating: 8.2 },
            { title: 'Attack on Titan', genre: 'Action', rating: 9.1 },
            { title: 'One Piece', genre: 'Adventure', rating: 8.8 },
            { title: 'Death Note', genre: 'Thriller', rating: 9.0 }
        ];
        let removedList = [];
        let selectedAnime = null;

        function renderTable() {
            const tableBody = document.querySelector('#animeTable tbody');
            tableBody.innerHTML = '';
            animeList.forEach((anime, index) => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${index + 1}</td>
                    <td>${anime.title}</td>
                    <td>${anime.genre}</td>
                    <td>${anime.rating}</td>
                `;
                row.onclick = () => selectAnime(anime);
                tableBody.appendChild(row);
            });
            document.getElementById('emptyMessage').style.display = animeList.length > 0 ? 'none' : 'block';
        }

        function addAnimeWithPrompt() {
            const title = prompt("Введите название аниме:");
            if (!title) return alert("Название не может быть пустым!");
            const genre = prompt("Введите жанр аниме:");
            if (!genre) return alert("Жанр не может быть пустым!");
            const rating = parseFloat(prompt("Введите рейтинг аниме (от 0 до 10):"));
            if (isNaN(rating) || rating < 0 || rating > 10) {
                return alert("Рейтинг должен быть числом от 0 до 10!");
            }
            animeList.push({ title, genre, rating });
            renderTable();
            alert("Новое аниме успешно добавлено!");
        }

        function removeAnime() {
            if (animeList.length > 0) {
                const removed = animeList.pop();
                removedList.push(removed);
                renderTable();
                alert(`Удалено: ${removed.title}`);
            } else {
                alert('Список пуст!');
            }
        }

        function restoreAnime() {
            if (removedList.length > 0) {
                const restored = removedList.pop();
                animeList.push(restored);
                renderTable();
                alert(`Восстановлено: ${restored.title}`);
            } else {
                alert('Нет удаленных элементов для восстановления.');
            }
        }

        function toggleTable() {
            const table = document.getElementById('animeTable');
            table.style.display = table.style.display === 'none' ? '' : 'none';
        }

        function selectAnime(anime) {
            selectedAnime = anime;
        }

        function openDialog() {
            if (selectedAnime) {
                document.getElementById('dialogTitle').textContent = selectedAnime.title;
                document.getElementById('dialogGenre').textContent = selectedAnime.genre;
                document.getElementById('dialogRating').textContent = selectedAnime.rating;
                document.getElementById('dialog').style.display = 'block';
            } else {
                alert("Выберите аниме для отображения информации.");
            }
        }

        function closeDialog() {
            document.getElementById('dialog').style.display = 'none';
        }

        renderTable();
    </script>
</body>
</html>
