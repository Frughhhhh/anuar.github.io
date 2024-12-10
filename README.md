<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Википедия о машинах">
    <title>Автомобильная Википедия</title>
    <link rel="stylesheet" type="text/css" href="css.css">
    <script src="https://code.jquery.com/jquery-3.6.4.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue@3/dist/vue.global.js"></script>
</head>
<body bgcolor="#808080" text="#000000">

<div id="app">
    <header>
        <h1>{{ title }}</h1>
    </header>

    <main>
        <div class="image-container">
            <img
                :src="carImage" 
                alt="Автомобиль" 
                align="right" 
                :width="imageWidth" 
                :height="imageHeight" 
                @mouseover="enlargeImage" 
                @mouseleave="resetImage"
            >
        </div>

        <div class="fixed"><h2>{{ mainPage }}</h2></div>

        <h2>Список популярных автомобилей</h2>
        <ul id="newListItem">
            <li v-for="(car, index) in cars" :key="index">
                <a :href="car.link" @click.prevent="selectCar(car.name)">{{ car.name }}</a>
            </li>
        </ul>
        <p>Список популярных автомобилей</p>
        <button @click="applyChanges">Применить изменения</button>
        <button id="redirectButton">Перейти на BMW</button>
        <button id="resizeTable">Увеличить таблицу</button>
        <button id="resetTable">Сбросить размеры таблицы</button>
        <button id="addRowButton">Добавить строку</button>

        <br><br>
        <br><br><br>

        <section>
            <h2 id="header">Таблица характеристик популярных машин</h2>
            <button id="colorButton">Изменить цвет заголовка</button>
            <button id="changeTitleButton">Изменить заголовок</button>
            <table id="resizableTable" v-show="showTable" border="1" align="center" width="90%" cellpadding="5" cellspacing="0">
                <caption>Характеристики автомобилей</caption>
                <thead>
                    <tr>
                        <th>Модель</th>
                        <th>Мощность (л.с.)</th>
                        <th>Тип топлива</th>
                        <th>Разгон (0-100 км/ч)</th>
                        <th>Максимальная скорость</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="car in carSpecs" :key="car.name">
                        <td>{{ car.name }}</td>
                        <td>{{ car.power }}</td>
                        <td>{{ car.fuelType }}</td>
                        <td>{{ car.acceleration }}</td>
                        <td>{{ car.topSpeed }}</td>
                    </tr>
                </tbody>
                <tfoot>
                    <tr>
                        <td colspan="5" align="center">Источник: cars.com</td>
                    </tr>
                </tfoot>
            </table>
        </section>

        <br><br>

        <section>
            <h2>Оставьте отзыв о сайте</h2>
            <form @submit.prevent="submitFeedback">
                <label for="name">Ваше имя:</label>
                <input type="text" id="name" v-model="feedback.name" required>

                <label for="email">Ваш email:</label>
                <input type="email" id="email" v-model="feedback.email" required>

                <label for="feedback">Ваш отзыв:</label>
                <textarea id="feedback" v-model="feedback.comment" rows="5" required></textarea>

                <input type="submit" value="Отправить">
            </form>
        </section>
    </main>

    <footer>
        <p align="center">© Автомобильная Википедия</p>
    </footer>
</div>

<script>
    const app = Vue.createApp({
        data() {
            return {
                title: "Википедия о машинах",
                mainPage: "Главная страница",
                carImage: "car.jpg",
                imageWidth: 700,
                imageHeight: 270,
                cars: [
                    { name: "BMW X5", link: "бмв.html" },
                    { name: "Mercedes-Benz C-Class", link: "мерседес.html" },
                    { name: "Audi A6", link: "ауди.html" }
                ],
                carSpecs: [
                    { name: "BMW X5", power: 394, fuelType: "Бензин", acceleration: "5.5 сек", topSpeed: "243 км/ч" },
                    { name: "Mercedes-Benz C-Class", power: 255, fuelType: "Бензин", acceleration: "6.0 сек", topSpeed: "250 км/ч" },
                    { name: "Audi A6", power: 335, fuelType: "Бензин", acceleration: "5.1 сек", topSpeed: "250 км/ч" }
                ],
                feedback: {
                    name: "",
                    email: "",
                    comment: ""
                },
                showTable: true,
            };
        },
        methods: {
            enlargeImage() {
                this.imageWidth = 750;
                this.imageHeight = 300;
            },
            resetImage() {
                this.imageWidth = 700;
                this.imageHeight = 270;
            },
            applyChanges() {
                this.cars.push({ name: "Lexus RX", link: "lexus.html" });
            },
            selectCar(carName) {
                alert(`Вы выбрали ${carName}`);
                localStorage.setItem("lastCar", carName);
            },
            submitFeedback() {
                alert(`Спасибо за отзыв, ${this.feedback.name}!`);
                this.feedback = { name: "", email: "", comment: "" };
            }
        },
        mounted() {
            const lastCar = localStorage.getItem("lastCar");
            if (lastCar) {
                alert(`Ваш последний выбор: ${lastCar}`);
            }
        }
    });

    app.mount("#app");

    $(document).ready(function () {
        $("#redirectButton                                                                                                                                                                                             ").click(function () {
            alert("Вы будете перенаправлены на страницу BMW!");
            window.location.href = "бмв.html";
        });
    });

    $(document).ready(function () {
        $("#redirectButton").css({
        "background-color": "#007BFF",
        "color": "#FFF",
        "border-radius": "5px",
        "padding": "10px 20px"
        });
    });

    $(document).ready(function () {
        $("header h1").css({
        "color": "#ff6347",
        "font-size": "40px",
        "font-family": "'Trebuchet MS', sans-serif"
        }).text("Автомобильная Википедия");
    });

    (function ($) {
            var defaults = { color: 'green' };
            var options;
            $.fn.myPlugin = function (params) {
                options = $.extend({}, defaults, options, params);

                return this.each(function () {
                    $(this).css('color', options.color);
                });
            };
        })(jQuery);

        $(document).ready(function () {
            $("#colorButton").click(function () {
                $("#header").mySimplePlugin({ color: 'blue' });
            });
        });

    $(document).ready(function () {
            $("#resizeTable").click(function () {
                $("#resizableTable").css({
                    "width": "100%",
                    "font-size": "18px"
                });
            });

            $("#resetTable").click(function () {
                $("#resizableTable").css({
                    "width": "100%",
                    "font-size": "14px"
                });
            });
        });

    function changeTitleWithJQuery(newTitle) {
    $("#header").text(newTitle);
    }

    $(document).ready(function () {
        $("#changeTitleButton").click(function () {
            changeTitleWithJQuery("Новый заголовок");
        });
    });

    (function($) {
    $.fn.addRowToTable = function(car) {
        const tableBody = this.find("tbody");

        const newRow = $("<tr>");

        const cells = [
            car.name,   
            car.power,   
            car.fuelType,
            car.acceleration, 
            car.topSpeed  
        ];

        cells.forEach(function(cellText) {
            $("<td>").text(cellText).appendTo(newRow);
        });

        tableBody.append(newRow);
    };
    })(jQuery);

    $(document).ready(function() {
    $("#addRowButton").click(function() {
        const car = {
            name: "Lexus RX 500h",
            power: 371,
            fuelType: "Гибрид",
            acceleration: "5.9 сек",
            topSpeed: "210 км/ч"
        };

        $("#resizableTable").addRowToTable(car);
    });
});



</script>

</body>
</html>
