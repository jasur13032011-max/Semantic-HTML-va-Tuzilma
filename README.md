# Semantic-HTML-va-Tuzilma
Veb-saytlardan klaviatura orqali foydalanish (Keyboard Accessibility) imkoniyati cheklangan va ekran o'quvchi dasturlardan foydalanuvchilar uchun hayotiy ahamiyatga ega. Keltirilgan 5 ta asosiy qoida va ularning amalga oshirilishi:

1. Tab va Escape tugmalari boshqaruvi
Tab: Foydalanuvchi klaviaturadagi Tab tugmasini bosganda, fokus faqat bosish mumkin bo'lgan elementlarga (havolalar, tugmalar, formalar) tartib bilan o'tishi kerak.

Escape: Har xil ochiluvchi elementlar (modal oynalar, akkordeonlar, dropdown menyular) Escape (Esc) tugmasi bosilganda darhol yopilishi shart.

2. Strelka tugmalari (Arrow Keys) bilan harakatlanish
Murakkab interfeyslar (masalan, tab-panellar, saytning bosh menyusi yoki dropdown ro'yxatlar) ichida harakatlanish uchun faqat Tabdan foydalanish noqulay.

Foydalanuvchi komponent ichiga Tab bilan kirgandan so'ng, uning ichidagi elementlar aro Chap/O'ng yoki Tepaga/Pastga strelka tugmalari orqali harakatlanishi to'g'ri hisoblanadi.

3. :focus-visible uslubi (Focus Indicator)
Klaviaturada harakatlanayotgan foydalanuvchi hozir sahifaning qaysi joyida turganini vizual tarzda ko'rib turishi shart.

:focus-visible CSS psevdo-klassi faqat klaviatura orqali fokuslanganda (sichqoncha bilan bosilganda emas) element atrofiga yorqin chiziq (outline) chizish imkonini beradi.

CSS
/* Sichqoncha bilan bosganda chiqmaydi, klaviaturada Tab bosilganda aniq ko'rinadi */
button:focus-visible, a:focus-visible {
  outline: 4px solid #3b82f6;
  outline-offset: 2px;
}
4. Skip Link (O'tkazib yuborish havolasi)
Sahifaning eng tepasida (odatda <body> ning ichida birinchi element bo'lib) yashirin havola joylashtiriladi.

Klaviaturadan foydalanuvchi har safar yangi sahifaga o'tganda o'sha bir xil menyularni qayta-qayta Tab bosib o'tmasligi uchun, ushbu havola uni to'g'ridan-to'g'ri asosiy kontentga (<main> blokiga) olib o'tadi. U odatda yashirin turadi va Tab bosilgandagina ko'rinadi.

HTML
<a href="#main-content" class="skip-link">Asosiy kontentga o'tish</a>
5. Fokus qopqoni (Focus Trap)
Modal oyna ochilganda eng muhim xavfsizlik qoidalaridan biri — fokusni modal oynaning ichida "asirga olish" (trap qilish) hisoblanadi.

Modal ochilganda, foydalanuvchi Tabni bosaversa ham fokus modal oynadan tashqariga (orqa fondagi elementlarga) chiqib ketmasligi kerak. Fokus oxirgi tugmaga yetsa va yana Tab bosilsa, u qaytadan modalning birinchi elementiga (masalan, yopish tugmasiga) qaytishi shart. Modal yopilgach esa, fokus modalni ochgan ilk tugmaga qaytariladi.

Amaliy JavaScript va HTML Namunasi (Focus Trap va Escape uchun):
HTML
<a href="#main" class="skip-link">Kontentga o'tish</a>

<button id="openModal">Modalni ochish</button>

<div id="myModal" role="dialog" aria-modal="true" style="display:none;">
  <button id="closeModal">Yopish</button>
  <input type="text" placeholder="Ismingiz">
  <button type="submit">Yuborish</button>
</div>

<script>
const openBtn = document.getElementById('openModal');
const modal = document.getElementById('myModal');
const closeBtn = document.getElementById('closeModal');

// 1. Escape orqali yopish
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape' && modal.style.display === 'block') {
    closeModalWindow();
  }
});

// 5. Fokus qopqoni (Focus Trap) logikasi
modal.addEventListener('keydown', (e) => {
  const focusableElements = modal.querySelectorAll('button, input');
  const firstElement = focusableElements[0];
  const lastElement = focusableElements[focusableElements.length - 1];

  if (e.key === 'Tab') {
    if (e.shiftKey) { // Shift + Tab orqaga harakatlanganda
      if (document.activeElement === firstElement) {
        lastElement.focus();
        e.preventDefault();
      }
    } else { // Shunchaki Tab oldinga harakatlanganda
      if (document.activeElement === lastElement) {
        firstElement.focus();
        e.preventDefault();
      }
    }
  }
});

function openModalWindow() {
  modal.style.display = 'block';
  closeBtn.focus(); // Modal ochilishi bilan birinchi elementga fokus beriladi
}

function closeModalWindow() {
  modal.style.display = 'none';
  openBtn.focus(); // 5. Yopilgach fokus ochgan tugmaga qaytadi
}

openBtn.addEventListener('click', openModalWindow);
closeBtn.addEventListener('click', closeModalWindow);
</script>
