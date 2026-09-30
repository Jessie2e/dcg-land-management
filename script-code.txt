const menuToggle = document.querySelector('.menu-toggle');
const siteNav = document.querySelector('.site-nav');

if (menuToggle && siteNav) {
  menuToggle.addEventListener('click', () => {
    const open = menuToggle.getAttribute('aria-expanded') === 'true';
    menuToggle.setAttribute('aria-expanded', String(!open));
    menuToggle.classList.toggle('is-open', !open);
    siteNav.classList.toggle('is-open', !open);
  });

  siteNav.querySelectorAll('a').forEach((link) => {
    link.addEventListener('click', () => {
      menuToggle.setAttribute('aria-expanded', 'false');
      menuToggle.classList.remove('is-open');
      siteNav.classList.remove('is-open');
    });
  });
}

document.querySelectorAll('[data-before-after]').forEach((component) => {
  const range = component.querySelector('.before-after__range');
  const beforeImage = component.querySelector('.before-after__img--before');
  const handle = component.querySelector('.before-after__handle');

  const update = () => {
    const value = `${range.value}%`;
    beforeImage.style.clipPath = `inset(0 ${100 - range.value}% 0 0)`;
    handle.style.left = value;
  };

  range.addEventListener('input', update);
  update();
});


const jobsiteVideo = document.querySelector('[data-jobsite-video]');
const videoToggle = document.querySelector('[data-video-toggle]');
const videoToggleText = document.querySelector('[data-video-toggle-text]');

if (jobsiteVideo && videoToggle && videoToggleText) {
  const playIcon = videoToggle.querySelector('[aria-hidden="true"]');
  const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)');
  const desktopPlayback = window.matchMedia('(min-width: 641px)');

  const syncVideoButton = () => {
    const isPaused = jobsiteVideo.paused;
    videoToggleText.textContent = isPaused ? 'Play video' : 'Pause video';
    videoToggle.setAttribute('aria-label', isPaused ? 'Play jobsite video' : 'Pause jobsite video');
    if (playIcon) playIcon.textContent = isPaused ? '▶' : 'Ⅱ';
  };

  const tryAutoplay = () => {
    if (desktopPlayback.matches && !prefersReducedMotion.matches) {
      jobsiteVideo.play().catch(() => syncVideoButton());
    } else {
      jobsiteVideo.pause();
    }
  };

  videoToggle.addEventListener('click', () => {
    if (jobsiteVideo.paused) {
      jobsiteVideo.play().catch(() => {});
    } else {
      jobsiteVideo.pause();
    }
  });

  jobsiteVideo.addEventListener('click', () => videoToggle.click());
  jobsiteVideo.addEventListener('play', syncVideoButton);
  jobsiteVideo.addEventListener('pause', syncVideoButton);

  if ('IntersectionObserver' in window) {
    const videoObserver = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          tryAutoplay();
        } else if (!jobsiteVideo.paused) {
          jobsiteVideo.pause();
        }
      });
    }, { threshold: 0.35 });
    videoObserver.observe(jobsiteVideo);
  } else {
    tryAutoplay();
  }

  desktopPlayback.addEventListener?.('change', tryAutoplay);
  prefersReducedMotion.addEventListener?.('change', tryAutoplay);
  syncVideoButton();
}

const form = document.querySelector('#estimate-form');
if (form) {
  form.addEventListener('submit', (event) => {
    event.preventDefault();
    const data = new FormData(form);
    const subject = `Estimate request from ${data.get('name') || 'website visitor'}`;
    const body = [
      `Name: ${data.get('name') || ''}`,
      `Phone: ${data.get('phone') || ''}`,
      `Email: ${data.get('email') || ''}`,
      `Location: ${data.get('location') || ''}`,
      `Service: ${data.get('service') || ''}`,
      '',
      'Project details:',
      data.get('message') || ''
    ].join('\n');

    window.location.href = `mailto:craig@dcgland.com?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
  });
}

const year = document.querySelector('#year');
if (year) year.textContent = new Date().getFullYear();

const revealItems = document.querySelectorAll('.reveal');
if ('IntersectionObserver' in window && !window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
  const observer = new IntersectionObserver((entries, obs) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('is-visible');
        obs.unobserve(entry.target);
      }
    });
  }, { threshold: 0.12 });
  revealItems.forEach((item) => observer.observe(item));
} else {
  revealItems.forEach((item) => item.classList.add('is-visible'));
}
