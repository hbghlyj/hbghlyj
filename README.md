// ==UserScript==
// @name         GitHub Markdown MathJax v4 Loader
// @namespace    http://tampermonkey.net/
// @version      1.0
// @description  Injects MathJax v4 to GitHub Markdown math renderer
// @match        https://github.com/*.md
// @grant        none
// @run-at       document-end
// ==/UserScript==
document.querySelectorAll('math-renderer').forEach(oldElement => {
    const newElement = document.createElement('tex');
    for (const attr of oldElement.attributes) {
      newElement.setAttribute(attr.name, attr.value);
    }
    newElement.innerHTML = oldElement.innerHTML.replaceAll('amp;','');
    oldElement.replaceWith(newElement);
});
window.MathJax = {
  startup: {
    elements: ['article']
  },
  tex: {
    inlineMath: {'[+]': [['$', '$']]}
  },
  options: {
    enableEnrichment: false
  }
};
const script = document.createElement('script');
script.src = 'https://cdn.jsdelivr.net/npm/mathjax@4/tex-svg.js';
script.async = true;
script.onerror = () => alert('Failed to load MathJax script.');
document.head.appendChild(script);
    const style = document.createElement('style');
    style.type = 'text/css';
const sheet = style.sheet;
(function() {
    // Create the <style> tag
    var style = document.createElement("style");

    // WebKit hack
    style.appendChild(document.createTextNode(""));

    // Add the <style> element to the page
    document.head.appendChild(style);
    console.log(style.sheet.cssRules); // length is 0, and no rules

    return style;
})().sheet.insertRule("mjx-container[display] { margin: 0 ! important}",0);
