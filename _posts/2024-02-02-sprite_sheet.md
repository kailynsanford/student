---
toc: true
comments: false
layout: post
title: sprite animation
description: ummm sprite!!!
type: tangibles
courses: { compsci: {week: 2} }
---

<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="fox_sprite" src="kailynsanford/student/images/fox_sprite.png">
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="idle" checked>
            <label for="idle">Idle</label><br>
            <input type="radio" name="animation" id="idle_look">
            <label for="idle_look">Idle Look</label><br>
            <input type="radio" name="animation" id="walking">
            <label for="walking">Walking</label><br>
        </div>
    </div>
</body>

<script>
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');  // sets the canvas as a variable by calling the canvas element from the HTML code, using the id we set
        const ctx = canvas.getContext('2d'); // the getContext function is a given function within the canvas object. It allows us more functionality with the sprite image.

        // constant variables used for sprite
        const SPRITE_WIDTH = 32;
        const SPRITE_HEIGHT = 32;
        const FRAME_LIMIT = 48;
       

        // sets canvas properties
        const SCALE_FACTOR = 5;
        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        //more code will be placed here later
        class Fox {
            constructor(){  // constructor method for outlining data points about the sprite
                this.image = document.getElementById("fox_sprite");
                this.spriteWidth = SPRITE_WIDTH;
                this.spriteHeight = SPRITE_HEIGHT;
                this.width = this.spriteWidth;  //takes sprite dimensions and accounts for it in image generation
                this.height = this.spriteHeight;
                this.x = 0;  //starts image generation in coordinate (0,0) px on sprite sheet
                this.y = 0;
                this.scale = 1;  //scale of image
                this.minFrame = 0;
                this.maxFrame = FRAME_LIMIT;  // how many sprites there are in a row
                this.frameX = 0;  // sets position of sprite that is being generated. There are the two parameters we will be changing in order to crete the animation. 
                this.frameY = 0;
            }

            draw(context) {
                context.drawImage(
                    this.image,
                    this.frameX * this.spriteWidth,
                    this.frameY * this.spriteHeight,
                    this.spriteWidth,
                    this.spriteHeight,
                    this.x,
                    this.y,
                    this.width * this.scale,
                    this.height * this.scale
                );
            } 
        }
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
        const fox = new Fox(); // makes game object fox

        const controls = document.getElementById("controls");
        controls.addEventListener('click',function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'idle':
                        fox.frameY= 32;
                        break;
                    case 'idle_look':
                        fox.frameY = 1;
                        break;
                    case 'walking':
                        fox.frameY = 2;
                        break;
                    default:
                        break:
                }
            }
        });
        function animate() {
            ctx.clearRect (0, 0, canvas.width, canvas.height);

            fox.draw(ctx);

            fox.update();

            requestAnimationFrame(animate);
        }

        animate();
    });

</script>