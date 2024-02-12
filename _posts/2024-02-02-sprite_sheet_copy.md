---
toc: true
comments: false
layout: post
title: sprite animation 2
description: ummm sprite!!!
type: tangibles
courses: { compsci: {week: 2} }
---

<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="fox_sprite" src="{{site.baseurl}}/images/fox_sprite.png">  // change sprite here
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="idle" checked>
            <label for="idle">Idle</label><br>
            <input type="radio" name="animation" id="idle_look">
            <label for="idle_look">Idle 2</label><br>
            <input type="radio" name="animation" id="walking">
            <label for="walking">Walking</label><br>
        </div>
    </div>
</body>

<script>
    // start on page load
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const ctx = canvas.getContext('2d');
        const SPRITE_WIDTH = 32;  // matches sprite pixel width
        const SPRITE_HEIGHT = 32; // matches sprite pixel height
        const FRAME_LIMIT = 14;  // matches number of frames per sprite row, this code assume each row is same

        const SCALE_FACTOR = 5;  // control size of sprite on canvas
        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        class Fox {
            constructor() {
                this.image = document.getElementById("fox_sprite");
                this.x = 0;
                this.y = 0;
                this.minFrame = 0;
                this.maxFrame = FRAME_LIMIT;
                this.frameX = 0;
                this.frameY = 0;
            }

            // draw fox object
            draw(context) {
                context.drawImage(
                    this.image,
                    this.frameX * SPRITE_WIDTH,
                    this.frameY * SPRITE_HEIGHT,
                    SPRITE_WIDTH,
                    SPRITE_HEIGHT,
                    this.x,
                    this.y,
                    canvas.width,
                    canvas.height
                );
            }

            // update frameX of object
            update() {
                if (this.frameX < this.maxFrame) {
                    this.frameX++;
                } else {
                    this.frameX = 0;
                }
            }
        }

        // fox object
        const fox = new Fox();

        // update frameY of Fox object, action from idle, bark, walk radio control
        const controls = document.getElementById('controls');
        controls.addEventListener('click', function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'idle':
                        fox.frameY = 0;
                        break;
                    case 'idle_look':
                        fox.frameY = 1;
                        break;
                    case 'walking':
                        fox.frameY = 2;
                        break;
                    default:
                        break;
                }
            }
        });

        // Animation recursive control function
        function animate() {
            // Clears the canvas to remove the previous frame.
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draws the current frame of the sprite.
            fox.draw(ctx);

            // Updates the `frameX` property to prepare for the next frame in the sprite sheet.
            fox.update();

            // Uses `requestAnimationFrame` to synchronize the animation loop with the display's refresh rate,
            // ensuring smooth visuals.
            requestAnimationFrame(animate);
        }

        // run 1st animate
        animate();
    });
</script>