---
title: Wiki.js
description: Usefull info
published: true
date: 2026-04-16T10:01:36.023Z
tags: wikijs
editor: markdown
dateCreated: 2026-04-16T10:01:36.023Z
---

# Sitemap
How to setup a sitemap as a regular page. When having a sitemap created earlier.

* Use HTML as media type

* Add this as content

```html
<h1>Sitemap</h1>
<div id="status">Loading sitemap\Uffffffff</div>
<div id="summary"></div>
<ul id="sitemap-list"></ul>
```

* Under page - scripts enter this and edit the url
```html
<script>
window.boot.register('page-ready', async () => {
  const statusEl = document.getElementById('status');
  const summaryEl = document.getElementById('summary');
  const listEl = document.getElementById('sitemap-list');

  if (!statusEl || !summaryEl || !listEl) {
    console.error('Missing target elements');
    return;
  }

  function esc(str) {
    return String(str).replace(/[&<>"']/g, m => ({
      '&': '&amp;',
      '<': '&lt;',
      '>': '&gt;',
      '"': '&quot;',
      "'": '&#039;'
    })[m]);
  }

  statusEl.textContent = 'Loading sitemap\Uffffffff';

  try {
    const res = await fetch('https://wiki.app.tuxmint.com/sitemap.xml', {
      method: 'GET',
      headers: {
        'Accept': 'application/xml, text/xml, */*'
      },
      cache: 'no-store'
    });

    if (!res.ok) {
      throw new Error(`HTTP ${res.status} ${res.statusText}`);
    }

    const xmlText = await res.text();
    console.log('sitemap.xml raw response:', xmlText);

    const xml = new DOMParser().parseFromString(xmlText, 'application/xml');

    if (xml.querySelector('parsererror')) {
      throw new Error('XML parse error');
    }

    const urlNodes = [...xml.querySelectorAll('url')];
    const sitemapNodes = [...xml.querySelectorAll('sitemap')];

    listEl.innerHTML = '';
    statusEl.textContent = '';

    if (urlNodes.length > 0) {
      summaryEl.textContent = `Found ${urlNodes.length} URLs`;

      for (const node of urlNodes) {
        const loc = node.querySelector('loc')?.textContent?.trim() || '';
        const lastmod = node.querySelector('lastmod')?.textContent?.trim() || '';

        const li = document.createElement('li');
        li.style.marginBottom = '0.75rem';
        li.innerHTML = `
          <div><a href="${esc(loc)}" target="_blank" rel="noopener noreferrer">${esc(loc)}</a></div>
          ${lastmod ? `<div>lastmod: ${esc(lastmod)}</div>` : ''}
        `;
        listEl.appendChild(li);
      }
    } else if (sitemapNodes.length > 0) {
      summaryEl.textContent = `Found ${sitemapNodes.length} sitemap entries`;

      for (const node of sitemapNodes) {
        const loc = node.querySelector('loc')?.textContent?.trim() || '';
        const lastmod = node.querySelector('lastmod')?.textContent?.trim() || '';

        const li = document.createElement('li');
        li.style.marginBottom = '0.75rem';
        li.innerHTML = `
          <div><a href="${esc(loc)}" target="_blank" rel="noopener noreferrer">${esc(loc)}</a></div>
          ${lastmod ? `<div>lastmod: ${esc(lastmod)}</div>` : ''}
        `;
        listEl.appendChild(li);
      }
    } else {
      summaryEl.textContent = 'No <url> or <sitemap> entries found';
    }
  } catch (err) {
    console.error('Sitemap load failed:', err);
    statusEl.textContent = 'Failed to load sitemap';
    summaryEl.textContent = err.message;
  }
});
</script>
```