# Semantic-HTML-va-Tuzilma
Keltirilgan 5 ta qoidaga asosan to'g'ri tarkibiy tuzilmani ko'rib chiqamiz:

1. <header>, <nav>, <main>, <footer> elementlari
Bu teglar sahifaning asosiy maketini (layout) belgilaydi:

<header>: Saytning yuqori qismi (bosh qismi). Odatda logo, sayt nomi va navigatsiyani ichiga oladi.

<nav>: Navigatsiya havolalari (menyu) bloki.

<main>: Sahifaning yagona, asosiy va takrorlanmas mazmuni.

<footer>: Sahifaning pastki qismi (zirh). Mualliflik huquqlari, aloqa ma'lumotlari va qo'shimcha havolalar joylashadi.

2. Sarlavhalar ierarxiyasi (<h1> → <h2> → <h3>)
Sarlavha teglari o'lcham uchun emas, matn muhimligini ko'rsatish uchun ishlatiladi:

<h1> sahifada faqat bitta bo'lishi va eng asosiy mavzuni ifodalashi kerak.

Sarlavha pog'onalarini tashlab ketish mumkin emas (masalan, <h1> dan keyin to'g'ridan-to'g'ri <h3> ga o'tib ketish xato).

3. <article> va <section> farqi
<article>: Mustaqil, alohida ko'chirib olinsa ham o'z ma'nosini yo'qotmaydigan kontent (masalan: blog posti, yangilik, forum mavzusi).

<section>: Sahifaning yoki biror maqolaning mantiqiy bo'limi. Odatda ichida o'zining sarlavhasi (<h2>-<h6>) bo'ladi.

4. <figure> va <figcaption>
Rasmlar, grafiklar yoki kod namunalarini tushuntirish matni bilan birga guruhlash uchun ishlatiladi:

<figure> rasm va uning matnini o'rab turadi.

<figcaption> rasmning ostidagi izoh matni hisoblanadi.

5. <nav> ichida <ul>/<li> ishlatish
Menyular ro'yxat shaklida bo'lgani sababli, ularni tartibsiz ro'yxat teglari orqali yozish semantik jihatdan eng to'g'ri yo'ldir.

To'g'ri tuzilgan HTML kod namunasi:
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>Semantik HTML Sahifa</title>
</head>
<body>

    <header>
        <h1>Mening Shaxsiy Blogim</h1>
        <nav>
            <ul>
                <li><a href="#home">Bosh sahifa</a></li>
                <li><a href="#articles">Maqolalar</a></li>
                <li><a href="#about">Biz haqimizda</a></li>
            </ul>
        </nav>
    </header>

    <main>
        
        <section id="articles">
            <h2>So'nggi Maqolalar</h2>

            <article>
                <h3>HTML5 Semantikasi haqida</h3> <p>Semantik teglar brauzer va dasturlarga sahifa mazmunini yaxshiroq tushunishga yordam beradi.</p>
                
                <figure>
                    <img src="html5-structure.png" alt="HTML5 tuzilishi sxemasi">
                    <figcaption>HTML5 sahifasining asosiy semantik bloklari sxemasi.</figcaption>
                </figure>
            </article>

            <article>
                <h3>CSS Grid nima?</h3>
                <p>CSS Grid orqali murakkab veb-maketlarni osonlik bilan yaratish mumkin.</p>
            </article>
        </section>

    </main>

    <footer>
        <p>&copy; 2026 Barcha huquqlar himoyalangan.</p>
    </footer>

</body>
</html>
