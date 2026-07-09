# Semantic-HTML-va-Tuzilma
Veb-saytingiz hamma uchun (shu jumladan ekran o'quvchi dasturlardan foydalanuvchi ko'zi ojizlar uchun ham) tushunarli bo'lishini ta'minlashda WAI-ARIA atributlarining o'rni juda katta. Keltirilgan 5 ta qoidaning amaliy qo'llanilishi va tushuntirishini ko'rib chiqamiz:

1. Accordion (Akkordeon)
Akkordeon bosilganda ochilib-yopiladigan blok hisoblanadi. Uni to'g'ri yaratish uchun quyidagi atributlar kerak:

aria-expanded="true/false": Tugmaga beriladi. Ichidagi kontent hozir ochiq (true) yoki yopiq (false) ekanini bildiradi. JavaScript yordamida dinamik o'zgartiriladi.

aria-controls="ID": Tugmaga beriladi va u boshqarayotgan kontent blokining idsini ko'rsatadi.

aria-hidden="true/false": Kontent blokiga beriladi. Blok yopiq bo'lganda true qilinadi, shunda ekran o'quvchi uni o'qimaydi.

2. Modal (Modal oyna)
Modal oyna ochilganda foydalanuvchi e'tiborini butunlay o'ziga qaratadigan elementdir:

role="dialog": Brauzerga bu shunchaki blok emas, balki muloqot oynasi (dialog) ekanini bildiradi.

aria-modal="true": Ekran o'quvchiga foydalanuvchi diqqatini faqat shu oyna ichida ushlab turishni, uning ortidagi elementlarga o'tib ketmaslikni buyuradi.

aria-labelledby="ID": Modal oynaning sarlavhasi (<h2>) idsiga bog'lanadi.

aria-describedby="ID": Modal oyna ichidagi qo'shimcha tushuntirish matni idsiga bog'lanadi.

3. Tugmalar (Buttons)
Tugma ichida nima vazifa bajarishi yozilgan matn bo'lishi shart.

Agar tugma ichida faqat rasm yoki belgi (masalan, +, ☰) bo'lsa, u holda aria-label orqali unga matnli tushuntirish berish majburiydir.

Agar tugma ichida "Sotib olish" kabi ko'rinadigan matn bo'lsa, aria-label shart emas.

4. Yopish tugmasi (Close Button)
Modal yoki bildirishnomalarni yopish uchun odatda ikonka (X belgisi) qo'yiladi. Ekran o'quvchi uni "Iks tugmasi" deb o'qimasligi uchun unga mantiqiy nom beriladi:

aria-label="Yopish" yoki inglizcha saytlarda aria-label="Close".

5. Dinamik o'zgarishlar (aria-live="polite")
Sahifa qayta yuklanmasdan turib biror matn o'zgarsa (masalan, savatga mahsulot qo'shilganda "Savatda 1 ta mahsulot bor" degan yozuv chiqsa), ko'zi ojiz foydalanuvchi buni sezmay qolishi mumkin.

aria-live="polite": Ekran o'quvchiga ushbu blokda o'zgarish bo'lganda, foydalanuvchining hozirgi o'qiyotgan matni tugashi bilan yangi o'zgargan matnni ham ovozli o'qib berishni buyuradi.

Amaliy HTML Kod Namunasi:
Quyidagi kodda yuqoridagi barcha qoidalar jamlangan:

HTML
<div class="accordion-item">
  <button id="acc-button" aria-expanded="false" aria-controls="acc-content">
    Ko'p beriladigan savollar
  </button>
  
  <div id="acc-content" aria-hidden="true">
    <p>Bu akkordeon ichidagi yashirin matn hisoblanadi.</p>
  </div>
</div>

<hr>

<div role="dialog" aria-modal="true" aria-labelledby="modal-title" aria-describedby="modal-desc">
  
  <h2 id="modal-title">Tizimga kirish</h2>
  <p id="modal-desc">Iltimos, profilingizga kirish uchun login va parolingizni kiriting.</p>
  
  <button type="button" aria-label="Yopish">&times;</button>
</div>

<hr>

<button type="button" aria-label="Qidirish">
  <svg>...</svg> </button>

<div aria-live="polite" id="cart-status">
  Savatda hozircha mahsulot yo'q.
</div>
