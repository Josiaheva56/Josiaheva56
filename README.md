1 // ==UserScript==
2 // @name         YouTube Dr. Tag
3 // @namespace    einarsnow
// @version      1.0
// @author       einarsnow
// @description  Tag user in YouTube live chat by click
// @updateURL    https://github.com/einarsnow/youtube-dr-tag/raw/main/youtube-dr-tag.user.js
// @downloadURL  https://github.com/einarsnow/youtube-dr-tag/raw/main/youtube-dr-tag.user.js
// @supportURL   https://github.com/einarsnow/youtube-dr-tag
// @match        https://www.youtube.com/live_chat*
// @grant        GM_addStyle
// ==/UserScript==

GM_addStyle(`
    #author-name {
        cursor: pointer;
    }
    #author-name:hover {
        text-decoration: underline;
    }
`)

const chat = document.querySelector("#chat")
chat.addEventListener("click", function(e) {
    if (e.target.id != "author-name") return

    let input = document.querySelector("#input[contenteditable]")
    input.innerText = `@${e.target.innerText}\xa0${input.innerText}`
    input.dispatchEvent(new Event("input"))

    const range = document.createRange()
    range.setStart(input, 1)
    range.collapse(true)
    const selection = window.getSelection()
    selection.removeAllRanges()
    selection.addRange(range)

    input.focus()
})
