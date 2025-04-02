# Agar-IO-Custom-Skin

Download a 512x512 image, not necessary but ok. Login to AgarIO PC, and login to your account. Open up the skin editor and press F12, go to console and paste this.

```
// ==UserScript==
// @name         New Userscript
// @namespace    http://tampermonkey.net/
// @version      2025-04-02
// @description  try to take over the world!
// @author       You
// @match        https://hibbard.eu/tampermonkey-tutorial/
// @icon         https://www.google.com/s2/favicons?sz=64&domain=hibbard.eu
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    function drawImageToCanvas(base64)
    {
        const canvas = document.getElementById("skin-editor-canvas");
        const context = canvas.getContext("2d");
        const image = new Image();
    
        image.onload = function () {
            canvas.width = 512;
            canvas.height = 512;
            context.drawImage(image, 0, 0, 512, 512);
            context.save();
        };
    
        image.src = base64;
    }

    function uploadImageProcess(event)
    {
        const file = event.target.files[0];
        const reader = new FileReader();

        reader.onloadend = function() {
            const base64 = reader.result;
            drawImageToCanvas(base64);
        };

        reader.readAsDataURL(file);
    }

    function checkButtonsList()
    {
        const leftTools = document.querySelector(".skin-editor-container");

        if (leftTools)
        {
            const button = document.createElement("input");
            button.textContent = "Upload Image";
            button.type = "file";
            button.accept = "image/*";
            button.id = "imageUploadCustom";

            leftTools.appendChild(button);

            button.addEventListener("change", uploadImageProcess);

            clearInterval(buttonCheckInterval);
        }
    }

    const buttonCheckInterval = setInterval(checkButtonsList, 1000);
})();
```
