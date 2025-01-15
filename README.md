// ==UserScript==
// @name         B1
// @namespace    http://tampermonkey.net/
// @version      1.4
// @description  B1
// @author       B1
// @match        https://www.bing.com/*
// @grant        none
// ==/UserScript==

(function () {
    'use strict';

    const searchWords = [
        "Astronomy", "Elephants", "Philosophy", "Volcanoes", "Origami",
        "Architecture", "Mythology", "Waterfalls", "Quantum", "Renaissance",
        "Glaciers", "Ecosystem", "Mathematics", "Agriculture", "Psychology",
        "Robotics", "Cartography", "Geography", "Linguistics", "Cryptography",
        "Literature", "Chemistry", "Biodiversity", "Horticulture",
        "Anthropology", "Archaeology", "Zoology", "Botany", "Ecology",
        "Technology", "Innovation", "Energy", "Sustainability", "Engineering",
        "Physics", "Mars", "Satellites", "AI", "Blockchain", "Big Data"
    ];

    const searchBox = document.querySelector('textarea.b_searchbox');
    const searchButton = document.querySelector('input.b_searchboxSubmit');
    if (!searchBox || !searchButton) return;

    const currentIndex = parseInt(localStorage.getItem('currentSearchIndex') || '0', 10);
    const nextWord = searchWords[currentIndex];
    const nextIndex = (currentIndex + 1) % searchWords.length;
    localStorage.setItem('currentSearchIndex', nextIndex);


    async function typeWord(word) {
        searchBox.value = "";
        for (const char of word) {
            searchBox.value += char;
            await delay(100 + Math.random() * 150);
        }
    }

    function delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }

    function randomScroll() {
        window.scrollTo({
            top: Math.random() * 500,
            behavior: 'smooth'
        });
    }

    async function performSearch() {
        await delay(2000 + Math.random() * 3000);
        await typeWord(nextWord);
        await delay(1000 + Math.random() * 2000);
        searchButton.click();
        randomScroll();
    }

    performSearch();
})();
