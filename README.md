# 🍪 Cookie Banner для WordPress (Vanilla JS + Inline CSS)

Лёгкий, адаптивный cookie-баннер для WordPress без зависимостей, реализованный на чистом JavaScript с инлайн-стилями. Подключается через `functions.php` и не требует внешних файлов.

---

## 🚀 Особенности

- ✅ Без jQuery и сторонних библиотек
- ✅ Стили вставляются инлайн в `<head>` — удобно для тестов и кастомизации
- ✅ Плавное появление и скрытие
- ✅ Локальное сохранение согласия через `localStorage`
- ✅ Семантические классы и адаптивная структура

---

## 📦 Установка

1. Скопируйте код в `functions.php` вашей темы.
2. Убедитесь, что `wp_head` и `wp_enqueue_scripts` не отключены.
3. Готово — баннер появится при первом посещении.

---

## 🧩 Структура баннера
```php
function farid_cookie_banner_script() {
  wp_register_script('cookie-banner', '', [], false, true);

  // Инлайн CSS
  $css = <<<CSS
:root {
  --ts-text-white: #ffffff;
  --ts-text-black: #110d19;
  --ts-color-blue: #46deb4;
  --ts-color-dark-blue: #346afe;
  --ts-text-background: linear-gradient(to top, var(--ts-color-blue) 40%, var(--ts-color-dark-blue) 100%);
  --ts-main-background: #110e19;
}
.cookie-banner p{
letter-spacing: 0px;
}
.cookie-banner {
  position: fixed;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%) translateY(100%);
  width: 100%;
  max-width: 1035px;
  margin: 0 auto;
  background: #fff;
  padding: 20px;
  border-radius: 0;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  z-index: 9999;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
  opacity: 0;
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.cookie-banner__inner {
  position: relative;
  width: 100%;
}

.cookie-banner__title {
  display: block;
  line-height: 26px;
  font-weight: 700;
  font-size: 16px;
  margin: 0 0 16px 0;
}

.cookie-banner__body {
  display: flex;
  gap: 20px;
  align-items: flex-start;
}

.cookie-banner__text {
  max-width: 820px;
  font-size: 14px;
  margin: 0;
  font-family: "Montserrat", sans-serif;
  font-weight: 400;
  line-height: 1.5;
}

.cookie-banner__link {
  font-size: 14px;
  text-decoration: underline;
  color: #000;
}

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  font-weight: 600;
  font-size: 16px;
  line-height: 20px;
  padding: 16px 50px;
  cursor: pointer;
  color: var(--ts-text-white);
  background: linear-gradient(to left, var(--ts-color-blue) 0, var(--ts-color-dark-blue) 100%);
  user-select: none;
  transition: opacity 0.3s;
}

.btn:hover {
  opacity: 0.8;
}

.cookie-banner__close {
  position: absolute;
  top: 5px;
  right: 5px;
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}
CSS;

  // Вставляем стили в <head>
  add_action('wp_head', function () use ($css) {
    echo '<style id="cookie-banner-style">' . $css . '</style>';
  });

  // JS-баннер
  $script = <<<JS
document.addEventListener('DOMContentLoaded', function () {
  if (localStorage.getItem('cookieAccepted')) return;

  const banner = document.createElement('div');
  banner.className = 'cookie-banner';

  banner.innerHTML = `
    <div class="cookie-banner__inner">
      <span class="cookie-banner__title">Файлы Cookie</span>

      <div class="cookie-banner__body">
        <p class="cookie-banner__text">
          Мы используем файлы Cookie, разработанные нашими специалистами и третьими лицами, для анализа событый на нашем веб-сайте, что позволяет нам улучшать взаимодействие с пользователями и обслуживание. Продолжая просмотр страниц нашего сайта, вы принимаете условия его использования. Более подробные сведения смотрите в нашей
          <a href="/privacy-policy" target="_blank" class="cookie-banner__link">Политике в отношении файлов Cookie</a>.
        </p>

        <p class="btn cookie-banner__accept">Принимаю</p>
      </div>

      <span class="cookie-banner__close">✕</span>
    </div>
  `;

  const acceptBtn = banner.querySelector(".cookie-banner__accept");
  const closeBtn = banner.querySelector(".cookie-banner__close");

  function hideBanner() {
    banner.style.opacity = '0';
    banner.style.transform = 'translateX(-50%) translateY(100%)';
    setTimeout(() => banner.remove(), 600);
  }

  acceptBtn.addEventListener("click", () => {
    localStorage.setItem("cookieAccepted", "true");
    hideBanner();
  });

  closeBtn.addEventListener("click", hideBanner);

  document.body.appendChild(banner);

  setTimeout(() => {
    banner.style.opacity = '1';
    banner.style.transform = 'translateX(-50%) translateY(0)';
  }, 50);
});
JS;

  wp_add_inline_script('cookie-banner', $script);
  wp_enqueue_script('cookie-banner');
}
add_action('wp_enqueue_scripts', 'farid_cookie_banner_script');
```
## 🧠 Поведение
- Баннер появляется с анимацией при загрузке страницы

- При клике на «Принимаю» или ✕ — скрывается и сохраняет cookieAccepted = true в localStorage

- При повторном посещении баннер не отображается

## 📁 Расширение
1. Добавьте автоскрытие через setTimeout

2. Подключите перевод через wp_localize_script

3. Вынесите стили в отдельный файл, если нужно кэширование

## 🛠 Автор
Разработано с любовью ✨ Farid 

## 📜 Лицензия
MIT — свободно используйте, адаптируйте и распространяйте.
