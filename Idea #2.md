# Idea #2
Our next idea is provided by me!
# What's the idea?
The idea is pong, but on the web.
# What Programming rank?
I would rate it for a Pro, or ultra beginner.
# Code
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width,initial-scale=1.0">
        <link rel="stylesheet" href="bootstrap.min.css">
        <link rel="stylesheet" href="arduino-light.min.css">
        <title>Web Pong</title>
        <script src="highlight.min.js"></script>
        <script src="javascript.min.js"></script>
        <style>
            canvas {
                display: block;
                background-color: #eee;
            }
            input {
                width: 100%;
            }
        </style>
    </head>
    <body>
        <canvas class="m-2" id="canvas" width="720" height="480"></canvas>
        <div class="card m-2">
            <div class="card-body">
                <h5 class="card-title">Instructions</h5>
                <p>Use <code>W</code> and <code>S</code> to move Player A (left) and <code>I</code> and <code>K</code> to move Player B (right)</p>
            </div>
        </div>
        <div class="card m-2">
            <form class="card-body">
                <h5 class="card-title">Hacker's Console</h5>
                <p><b>To:</b> Hack This Game!</p>
                <p class="d-inline-flex gap-1">
                    <button class="btn btn-primary" type="button" data-bs-toggle="collapse" data-bs-target="#help">
                        Examples
                    </button>
                </p>
                <div class="collapse" id="help">
                    <div class="card card-body">
                        <h6>Change Score</h6>
                        <pre><code class="language-js">scoreA = 1; // Change score of player A
scoreB = 1; // Change score of player B</code></pre>
                        <h6>Change Paddle Properties</h6>
                        <pre><code class="language-js">// You can switch the paddles!
// in these examples.
paddleA.height = 10; // Change Height
paddleA.width = 10; // Change Width
paddleA.x = 10; // Change X! (breaks the game!)
paddleA.y = 10; // Change Y</code></pre>
                        <h6> Ball Properties</h6>
                        <pre><code class="language-js">x = 10; // x 
y = 10; // y 
dx = 5; // horizontal speed 
dy = 5; // vertical speed </code></pre>
                        <h6>Useful Constants & Functions</h6>
                        <pre><code class="language-js">canvas // HTML canvas element
canvas.width // Canvas width
canvas.height // Canvas height

alert() // Display stuff using an annoying popup window.
document.write() // Display stuff, but replaces the whole website.</code></pre>
                    </div>
                </div>
                <input id="terminal" class="form-control mt-1 mb-1">
                <button id="execute" type="button" class="btn btn-success mb-1 mt-1">Hack!</button>
            </form>
        </div>
        <script>
            // Canvas constants
            const canvas = document.getElementById("canvas");
            const ctx = canvas.getContext("2d");

            // Scores
            let scoreA = 0;
            let scoreB = 0;

            // Ball variables
            const ballRadius = 10;
            let x = canvas.width / 2;
            let y = Math.floor(Math.random() * (canvas.height - 20)) + 10;
            let dx = 4;
            let dy = -4;
            
            // Paddle constructor
            function Paddle(height, width, x, upKey, downKey) {
                // Properties
                this.height = height;
                this.width = width;
                this.x = x;
                this.y = canvas.height / 2 - this.height / 2;

                // Key pressed
                this.upPressed = false;
                this.downPressed = false;

                // Functions
                this.draw = () => {
                    if (this.downPressed) this.y = Math.min(this.y + 7, canvas.height - this.height);
                    if (this.upPressed) this.y = Math.max(this.y - 7, 0);

                    ctx.beginPath();
                    ctx.rect(this.x, this.y, this.width, this.height);
                    ctx.fillStyle = "#808080";
                    ctx.fill();
                    ctx.closePath();
                };
                
                // Event handlers
                document.addEventListener("keydown", (e) => {
                    if (e.key == upKey) {
                        this.upPressed = true;
                    } else if (e.key == downKey) {
                        this.downPressed = true;
                    }
                }, false);

                document.addEventListener("keyup", (e) => {
                    if (e.key == upKey) {
                        this.upPressed = false;
                    } else if (e.key == downKey) {
                        this.downPressed = false;
                    }
                }, false);
            }

            // Paddles
            const paddleA = new Paddle(100, 10, 0, "w", "s");
            const paddleB = new Paddle(100, 10, canvas.width - 10, "i", "k");

            function drawBall() {
                // Draw the ball
                ctx.beginPath();
                ctx.arc(x, y, ballRadius, 0, Math.PI * 2);
                ctx.fillStyle = "#FF0000";
                ctx.fill();
                ctx.closePath();
                
                // Update the ball's position
                x += dx;
                y += dy;

                // Bounce the ball if it hits the edge of the canvas
                if (y + dy < ballRadius) dy = -dy;
                if (y + dy > canvas.height - ballRadius) dy = -dy;

                if (x + dx < ballRadius || x + dx > canvas.width - ballRadius) {
                    if (y > paddleA.y && y < paddleA.y + paddleA.height && x + dx < ballRadius || // FIX BUG!
                        y > paddleB.y && y < paddleB.y + paddleB.height && x + dx > canvas.width - ballRadius) {
                        dx = -dx;
                    } else {
                        if (x + dx < ballRadius) {
                            scoreB++;
                            restartBall();
                        } else {
                            scoreA++;
                            restartBall();
                        }
                    }
                }
            }

            // Restarts the ball
            function restartBall() {
                x = canvas.width / 2;
                y = Math.floor(Math.random() * canvas.height);
                dx = 4;
                dy = -4;
            }

            // Draws the score
            function drawScore() {
                ctx.font = "16px Arial";
                ctx.fillStyle = "#000000";
                ctx.fillText(`Player A: ${scoreA}`, 8, 20);
                ctx.fillText(`Player B: ${scoreB}`, 8, 40);
            }

            function draw() {
                // Clear the canvas
                ctx.clearRect(0, 0, canvas.width, canvas.height);

                // Draw the score
                drawScore();

                // Draw the ball
                drawBall();

                // Draw the paddles
                paddleA.draw();
                paddleB.draw();

                // Request an animation frame
                requestAnimationFrame(draw);
            }

            draw();

            // Terminal
            document.getElementById("execute").addEventListener("click", function() {
                const value = document.getElementById("terminal").value;
                eval(value);
            });

            // Reroute enter key presses to click the "Hack!" button
            document.querySelector("form").addEventListener("submit", function(e) {
                e.preventDefault();
                document.getElementById("execute").click();
            })

            window.onload = () => { hljs.highlightAll() };
        </script>
        <script src="bootstrap.min.js"></script>
    </body>
</html>
```
# For it:
<p><a href="http://eternalxlks.github.io/WebPong/">Web Pong!</a></p>