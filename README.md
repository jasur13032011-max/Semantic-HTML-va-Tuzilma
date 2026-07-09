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
Klaviatura navigatsiyasi va barcha ARIA talablariga to'liq javob beradigan, real loyihalarda ishlatishga tayyor Dropdown menyu va Modal oyna (Focus Trap bilan) kodi.

Bu kodda darsda aytilgan barcha shartlar (Tab, Escape, strelka tugmalari, :focus-visible, skip-link, va focus trap) to'liq qamrab olingan.

HTML va CSS kodi
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mukammal Aksessibilliti Sahifasi</title>
  <style>
    /* 3. :focus-visible uslubi - Sichqonchada ko'rinmaydi, klaviaturada yorqin ajralib turadi */
    :focus-visible {
      outline: 3px solid #0056b3;
      outline-offset: 3px;
    }

    /* 4. Skip link uslubi - Oddiy holatda yashirin, fokus tushganda tepada chiqadi */
    .skip-link {
      position: absolute;
      top: -100px;
      left: 10px;
      background: #007bff;
      color: white;
      padding: 10px 20px;
      z-index: 100;
      text-decoration: none;
      transition: top 0.2s ease;
    }
    .skip-link:focus-visible {
      top: 10px;
    }

    /* Dropdown uslublari */
    .dropdown {
      position: relative;
      display: inline-block;
      margin: 20px;
    }
    .dropdown-menu {
      display: none;
      position: absolute;
      top: 100%;
      left: 0;
      background: white;
      border: 1px solid #ccc;
      list-style: none;
      padding: 5px 0;
      margin: 0;
      min-width: 160px;
    }
    .dropdown-menu li a {
      display: block;
      padding: 8px 16px;
      color: #333;
      text-decoration: none;
    }
    .dropdown-menu li a:focus {
      background-color: #f1f1f1;
    }
    .show { display: block; }

    /* Modal uslublari */
    .modal-overlay {
      position: fixed;
      top: 0; left: 0; right: 0; bottom: 0;
      background: rgba(0,0,0,0.5);
      display: none;
      justify-content: center;
      align-items: center;
    }
    .modal-content {
      background: white;
      padding: 30px;
      border-radius: 8px;
      max-width: 400px;
      width: 100%;
      position: relative;
    }
    .open-modal-flex { display: flex; }
  </style>
</head>
<body>

  <a href="#main-content" class="skip-link">Asosiy kontentga o'tish</a>

  <header>
    <nav aria-label="Asosiy menyu">
      <div class="dropdown">
        <button id="dropdown-btn" aria-haspopup="true" aria-expanded="false" aria-controls="dropdown-items">
          Xizmatlar <span aria-hidden="true">▼</span>
        </button>
        <ul id="dropdown-items" class="dropdown-menu" role="menu" aria-labelledby="dropdown-btn">
          <li role="none"><a href="#web" role="menuitem">Veb dasturlash</a></li>
          <li role="none"><a href="#design" role="menuitem">Grafik dizayn</a></li>
          <li role="none"><a href="#seo" role="menuitem">SEO optimizatsiya</a></li>
        </ul>
      </div>
    </nav>
  </header>

  <main id="main-content">
    <h1>Mukammal navigatsiya testi</h1>
    <button id="open-modal-btn">Modal oynani ochish</button>
  </main>

  <div id="modal-el" class="modal-overlay" role="dialog" aria-modal="true" aria-labelledby="modal-title" hidden>
    <div class="modal-content">
      <h2 id="modal-title">Tizim bildirishnomasi</h2>
      <p>Bu yerda muhim ma'lumot joylashgan. Fokus shu oyna ichida qulflanadi.</p>
      
      <input type="text" placeholder="Ismingizni kiriting" aria-label="Ism">
      <button type="button">Saqlash</button>
      
      <button id="close-modal-btn" aria-label="Yopish">&times;</button>
    </div>
  </div>

  <script>
    // ==========================================
    // DROPDOWN NAVIGATSIYASI (Strelkalar va Esc)
    // ==========================================
    const dropdownBtn = document.getElementById('dropdown-btn');
    const dropdownMenu = document.getElementById('dropdown-items');
    const menuItems = dropdownMenu.querySelectorAll('[role="menuitem"]');
    
    let currentIndex = -1;

    function toggleDropdown(open) {
      dropdownBtn.setAttribute('aria-expanded', open);
      dropdownMenu.classList.toggle('show', open);
      if (!open) currentIndex = -1;
    }

    dropdownBtn.addEventListener('click', () => {
      const isOpen = dropdownBtn.getAttribute('aria-expanded') === 'true';
      toggleDropdown(!isOpen);
    });

    // 2. Strelka va 1. Escape tugmalari boshqaruvi
    dropdownBtn.addEventListener('keydown', (e) => {
      if (e.key === 'ArrowDown') {
        e.preventDefault();
        toggleDropdown(true);
        currentIndex = 0;
        menuItems[currentIndex].focus();
      }
    });

    dropdownMenu.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') {
        toggleDropdown(false);
        dropdownBtn.focus();
      } else if (e.key === 'ArrowDown') {
        e.preventDefault();
        currentIndex = (currentIndex + 1) % menuItems.length;
        menuItems[currentIndex].focus();
      } else if (e.key === 'ArrowUp') {
        e.preventDefault();
        currentIndex = (currentIndex - 1 + menuItems.length) % menuItems.length;
        menuItems[currentIndex].focus();
      }
    });

    // Tashqariga bosganda yopish
    document.addEventListener('click', (e) => {
      if (!dropdownBtn.contains(e.target) && !dropdownMenu.contains(e.target)) {
        toggleDropdown(false);
      }
    });


    // ==========================================
    // MODAL OYNA VA FOKUS QOPQONI (Focus Trap)
    // ==========================================
    const openModalBtn = document.getElementById('open-modal-btn');
    const modalEl = document.getElementById('modal-el');
    const closeModalBtn = document.getElementById('close-modal-btn');
    
    // Fokuslanuvchi elementlarni olish
    const focusableElementsString = 'button, input, [tabindex="0"]';
    
    function openModal() {
      modalEl.removeAttribute('hidden');
      modalEl.classList.add('open-modal-flex');
      
      const focusableEls = modalEl.querySelectorAll(focusableElementsString);
      if (focusableEls.length > 0) {
        // 5. Modal ochilishi bilan birinchi elementga fokus boradi
        focusableEls[0].focus(); 
      }
    }

    function closeModal() {
      modalEl.setAttribute('hidden', 'true');
      modalEl.classList.remove('open-modal-flex');
      // 5. Yopilgandan keyin fokus uni ochgan tugmaga qaytadi
      openModalBtn.focus(); 
    }

    openModalBtn.addEventListener('click', openModal);
    closeModalBtn.addEventListener('click', closeModal);

    // 1. Escape orqali yopish va 5. Fokus qopqoni logikasi
    document.addEventListener('keydown', (e) => {
      if (modalEl.hasAttribute('hidden')) return;

      if (e.key === 'Escape') {
        closeModal();
      }

      if (e.key === 'Tab') {
        const focusableEls = modalEl.querySelectorAll(focusableElementsString);
        const firstEl = focusableEls[0];
        const lastEl = focusableEls[focusableEls.length - 1];

        if (e.shiftKey) { // Shift + Tab (orqaga)
          if (document.activeElement === firstEl) {
            lastEl.focus();
            e.preventDefault();
          }
        } else { // Shunchaki Tab (oldinga)
          if (document.activeElement === lastEl) {
            firstEl.focus();
            e.preventDefault();
          }
        }
      }
    });
  </script>
</body>
</html>
Kod qanday ishlaydi?
Skip Link: Sahifa ochilib Tab bosilganda ekranning chap yuqori qismida "Asosiy kontentga o'tish" havolasi paydo bo'ladi.

Dropdown (Strelkalar): "Xizmatlar" tugmasida ArrowDown (Pastga strelka) bosilsa menyu ochilib, fokus birinchi elementga o'tadi. ArrowUp/ArrowDown orqali menyu ichida aylanadi. Esc bosilsa yopilib, fokus yana asosiy tugmaga qaytadi.

Focus Trap (Modal): Modal ochilganda fokus uning tashqarisiga chiqa olmaydi. Tab bosilaversa, modal ichidagi oxirgi tugmadan yana birinchi elementga aylanib yuraveradi.
