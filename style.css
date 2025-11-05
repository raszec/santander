// script.js - mobile-friendly overlay + focus trap + swipe to close
document.addEventListener('DOMContentLoaded', function () {
  const openBtn = document.getElementById('openMenu');
  const closeBtn = document.getElementById('closeMenu');
  const overlay = document.getElementById('overlay');
  const overlayBg = document.getElementById('overlayBg');
  const sidepanel = document.querySelector('.sidepanel');

  // find focusable elements inside panel
  function getFocusable(container) {
    if (!container) return [];
    return Array.from(container.querySelectorAll('a, button, textarea, input, select, [tabindex]:not([tabindex="-1"])'))
      .filter(el => !el.disabled && el.offsetParent !== null);
  }

  function openMenu() {
    overlay.classList.add('show');
    overlay.setAttribute('aria-hidden', 'false');
    document.body.style.overflow = 'hidden';
    // set focus to first focusable inside
    const focusable = getFocusable(sidepanel);
    if (focusable.length) focusable[0].focus();
    // prevent background from being scrollable on iOS
    document.documentElement.style.position = 'fixed';
    document.documentElement.style.width = '100%';
  }

  function closeMenu() {
    overlay.classList.remove('show');
    overlay.setAttribute('aria-hidden', 'true');
    document.body.style.overflow = '';
    // restore html positioning
    document.documentElement.style.position = '';
    document.documentElement.style.width = '';
    // return focus
    openBtn.focus();
  }

  // events
  openBtn.addEventListener('click', openMenu);
  closeBtn.addEventListener('click', closeMenu);
  overlayBg.addEventListener('click', closeMenu);

  // keyboard handling (ESC + focus trap)
  document.addEventListener('keydown', function (ev) {
    if (ev.key === 'Escape') {
      if (overlay.classList.contains('show')) closeMenu();
    }

    if (ev.key === 'Tab' && overlay.classList.contains('show')) {
      const focusable = getFocusable(sidepanel);
      if (focusable.length === 0) {
        ev.preventDefault();
        return;
      }
      const first = focusable[0];
      const last = focusable[focusable.length - 1];
      if (ev.shiftKey) {
        if (document.activeElement === first) {
          last.focus();
          ev.preventDefault();
        }
      } else {
        if (document.activeElement === last) {
          first.focus();
          ev.preventDefault();
        }
      }
    }
  });

  // swipe to close (touch)
  let touchStartX = 0;
  let touchStartY = 0;
  let isSwiping = false;

  sidepanel.addEventListener('touchstart', (e) => {
    if (!e.touches || e.touches.length === 0) return;
    touchStartX = e.touches[0].clientX;
    touchStartY = e.touches[0].clientY;
    isSwiping = true;
  }, {passive: true});

  sidepanel.addEventListener('touchmove', (e) => {
    if (!isSwiping || !e.touches || e.touches.length === 0) return;
    const dx = e.touches[0].clientX - touchStartX;
    const dy = Math.abs(e.touches[0].clientY - touchStartY);
    // horizontal swipe right and mostly horizontal
    if (dx > 40 && dy < 80) {
      closeMenu();
      isSwiping = false;
    }
  }, {passive: true});

  sidepanel.addEventListener('touchend', () => { isSwiping = false; });

  // Ensure clicking any menu button also closes overlay if appropriate (optional)
  sidepanel.addEventListener('click', (e) => {
    const target = e.target.closest('button, a');
    if (!target) return;
    // if clicked an inner nav or button (not the close), optionally close for small devices
    if (target.classList.contains('icon-item') || target.tagName.toLowerCase() === 'a') {
      // closeMenu(); // comment/uncomment depending on desired behavior
    }
  });

  // Prevent focus escape when overlay hidden
  const observer = new MutationObserver(() => {
    if (!overlay.classList.contains('show')) return;
    // ensure first element focusable
    const focusable = getFocusable(sidepanel);
    if (focusable.length) focusable[0].setAttribute('tabindex', '0');
  });
  observer.observe(overlay, { attributes: true, attributeFilter: ['class'] });
});
