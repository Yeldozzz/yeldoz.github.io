<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Аниме и Манга</title>
  
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    
   
    <link rel="stylesheet" href="https://code.jquery.com/ui/1.12.1/themes/base/jquery-ui.css">
    <script src="https://code.jquery.com/ui/1.12.1/jquery-ui.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
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
        button {
            margin: 5px;
            padding: 8px 12px;
            cursor: pointer;
        }
     
        #dialog {
            display: none;
        }
    </style>
</head>
<body>
    <div id="animeApp">
        <h1>Список Аниме и Манги</h1>

        <div>
            <button @click="addAnimeWithPrompt">Добавить новое аниме</button>
            <button @click="removeAnime">Удалить последнее аниме</button>
            <button @click="restoreAnime">Восстановить последнее удаленное</button>
            <button id="toggleList">Показать/Скрыть таблицу</button>
    
            <button id="showDialog" @click="openDialog">Показать информацию</button>
        </div>

        <table v-if="animeList.length > 0">
            <thead>
                <tr>
                    <th>№</th>
                    <th>Название</th>
                    <th>Жанр</th>
                    <th>Рейтинг</th>
                    <th>Действия</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(anime, index) in animeList" :key="index">
                    <td>{{ index + 1 }}</td>
                    <td>{{ anime.title }}</td>
                    <td>{{ anime.genre }}</td>
                    <td>{{ anime.rating }}</td>
                    <td><button @click="selectAnime(anime)">Выбрать</button></td>
                </tr>
            </tbody>
        </table>
        <p v-else>Список пуст</p>

        <div v-if="selectedAnime" id="dialog" title="Информация о аниме">
            <p><strong>Название:</strong> {{ selectedAnime.title }}</p>
            <p><strong>Жанр:</strong> {{ selectedAnime.genre }}</p>
            <p><strong>Рейтинг:</strong> {{ selectedAnime.rating }}</p>
        </div>

        <button id="dlyaCSS">Изменить стили и контент</button>
    </div>

    <script>
        
        const app = new Vue({
            el: '#animeApp',
            data: {
                animeList: [
                    { title: 'Naruto', genre: 'Shounen', rating: 8.2 },
                    { title: 'Attack on Titan', genre: 'Action', rating: 9.1 },
                    { title: 'One Piece', genre: 'Adventure', rating: 8.8 },
                    { title: 'Death Note', genre: 'Thriller', rating: 9.0 },
                    { title: 'My Hero Academia', genre: 'Superhero', rating: 8.4 },
                    { title: 'Tokyo Ghoul', genre: 'Horror', rating: 7.9 },
                    { title: 'Sword Art Online', genre: 'Fantasy', rating: 7.5 },
                    { title: 'Fullmetal Alchemist', genre: 'Fantasy', rating: 9.2 }
                ],
                removedList: [], 
                newAnime: { title: '', genre: '', rating: '' }, 
                selectedAnime: null
            },
            methods: {
                addAnimeWithPrompt() {
                  const title = prompt("Введите название аниме:");
                  if (!title) return alert("Название не может быть пустым!");
                  const genre = prompt("Введите жанр аниме:");
                  if (!genre) return alert("Жанр не может быть пустым!");
                  const rating = parseFloat(prompt("Введите рейтинг аниме (от 0 до 10):"));
                  if (isNaN(rating) || rating < 0 || rating > 10) {
                      return alert("Рейтинг должен быть числом от 0 до 10!");
                  }
                  this.animeList.push({ title, genre, rating });
                  alert("Новое аниме успешно добавлено!");
                },
                removeAnime() {
                    if (this.animeList.length > 0) {
                        const removed = this.animeList.pop();
                        this.removedList.push(removed);
                        alert(`Удалено: ${removed.title}`);
                    } else {
                        alert('Список пуст!');
                    }
                },
                restoreAnime() {
                    if (this.removedList.length > 0) {
                        const restored = this.removedList.pop();
                        this.animeList.push(restored);
                        alert(`Восстановлено: ${restored.title}`);
                    } else {
                        alert('Нет удаленных элементов для восстановления.');
                    }
                },
                addCustomAnime() {
                    if (this.newAnime.title && this.newAnime.genre && this.newAnime.rating) {
                        this.animeList.push({ ...this.newAnime });
                        this.newAnime = { title: '', genre: '', rating: '' };
                    } else {
                        alert('Пожалуйста, заполните все поля.');
                    }
                },
                selectAnime(anime) {
                    this.selectedAnime = anime;
                },
                openDialog() {
                    if (this.selectedAnime) {
                        $('#dialog').dialog({
                            modal: true,
                            buttons: {
                                "Закрыть": function() {
                                    $(this).dialog("close");
                                }
                            }
                        });
                    } else {
                        alert("Выберите аниме для отображения информации.");
                    }
                }
            }
        });

        $(document).ready(function () {
            $('#toggleList').click(function () {
                $('table').fadeToggle();
            });

            $('#dlyaCSS').click(function() {
          
                $('table').css({
                    'background-color': 'grey',
                    'border': '2px solid #ccc',
                });

                $('h1').text('Список Аниме и Манги - Обновленный');

                $('button').css({
                    'background-color': 'green',
                    'color': 'white',
                    'font-weight': 'bold'
                });

                $('#animeApp').append('<p style="color: white;">Все изменения успешно применены!</p>');
            });

            $('#toggleList').bind('click', function() {
                alert('Таблица будет показана или скрыта!');
            });

            
        });
        
    </script>
</body>
</html>
