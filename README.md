// ==UserScript==
// @name         B3
// @namespace    http://tampermonkey.net/
// @version      0.3
// @description  b3
// @author       B3
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

    function getRandomDelay(min, max) {
        return Math.floor(Math.random() * (max - min + 1) + min);
    }

    async function typeWord(searchBox, word) {
        searchBox.value = "";
        for (const char of word) {
            searchBox.value += char;
            await delay(getRandomDelay(100, 300));
        }
    }

    function delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }

    function randomScroll() {
        const scrollHeight = document.body.scrollHeight;
        const randomPosition = Math.random() * scrollHeight * 0.5;
        window.scrollTo({ top: randomPosition, behavior: 'smooth' });
    }

    function highlightWord(word) {
        const regex = new RegExp(word, "gi");
        document.body.innerHTML = document.body.innerHTML.replace(regex, match => {
            const color = Math.random() > 0.5 ? 'red' : 'green';
            return `<span style='color:${color};'>${match}</span>`;
        });
    }

    async function performSearch() {
        const searchBox = document.querySelector('textarea.b_searchbox');
        const searchButton = document.querySelector('input.b_searchboxSubmit');
        if (!searchBox || !searchButton) return;

        const currentIndex = parseInt(localStorage.getItem('currentSearchIndex') || '0', 10);
        const nextWord = searchWords[currentIndex];
        const nextIndex = (currentIndex + 1) % searchWords.length;
        localStorage.setItem('currentSearchIndex', nextIndex);

        await delay(getRandomDelay(2000, 5000));
        await typeWord(searchBox, nextWord);
        await delay(getRandomDelay(1500, 3000));
        searchButton.click();
        randomScroll();

        setTimeout(() => {
            highlightWord(nextWord);
            setTimeout(() => {
                window.location.reload();
            }, getRandomDelay(8000, 15000));
        }, getRandomDelay(5000, 10000));
    }

    performSearch().catch(err => console.error("Error in script execution:", err));
})();
