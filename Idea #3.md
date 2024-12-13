# Idea #3
Yeah, a new idea that is a bomb dodgers game.
# Code:
```
<!DOCTYPE html>
<html>
    <head>
        <title>Bomb Dodgers</title>
        <script src="phaser.min.js"></script>
    </head>
    <body>
        <script>
            var config = {
                type: Phaser.AUTO,
                width: 800,
                height: 600,
                physics: {
                    default: "arcade",
                    arcade: {
                        gravity: { y: 600 },
                        debug: false
                    }
                },
                scene: {
                    preload,
                    create,
                    update
                }
            }

            var platforms;
            var player;
            var bombs;
            var bombCounter = 0;

            var game = new Phaser.Game(config)

            function preload() {
                this.load.image("sky", "sigma.png")
                this.load.image("grass", "grass.png")
                this.load.image("player", "player.png")
                this.load.image("bomb", "bomb.png")
            }

            function create() {
                this.add.image(400, 300, "sky")

                platforms = this.physics.add.staticGroup()

                for (let i = 16; i < 800; i += 32) {
                    platforms.create(i, 584, "grass")
                }

                for (let i = 16; i < 200; i += 32) {
                    platforms.create(i, 450, "grass")
                }

                platforms.create(400, 350, "grass")

                for (let i = 784; i > 600; i -= 32) {
                    platforms.create(i, 250, "grass")
                }

                for (let i = 784; i > 700; i -= 32) {
                    platforms.create(i, 400, "grass")
                }

                player = this.physics.add.sprite(16, 500, "player")

                player.setBounce(0.2)
                player.setCollideWorldBounds(true)

                this.physics.add.collider(player, platforms)

                bombs = this.physics.add.group()
                this.physics.add.collider(bombs, player)
                this.physics.add.collider(player, bombs, () => { this.physics.pause() }, null, this)
            }

            function update() {
                let keyA = this.input.keyboard.addKey(Phaser.Input.Keyboard.KeyCodes.A)
                let keyD = this.input.keyboard.addKey(Phaser.Input.Keyboard.KeyCodes.D)

                let keyW = this.input.keyboard.addKey(Phaser.Input.Keyboard.KeyCodes.W)

                if (keyA.isDown) {
                    player.setVelocityX(-300) 
                }
                if (keyD.isDown) {
                    player.setVelocityX(300)
                }
                if (keyW.isDown && player.body.touching.down) {
                    player.setVelocityY(-500)
                }

                if (!keyA.isDown && !keyD.isDown) {
                    player.setVelocityX(0)
                }

                bombCounter++

                if (bombCounter ==30) {
                    bombCounter = 0

                    var bomb = bombs.create(player.x + (Math.random() - 0.5) * 50, 0, "bomb")
                }
            }
        </script>
    </body>
</html>
```
# What Else?
[The Actual Game](https://eternalxlks.github.io/BombDodgers)
And good luck finding the images and placing them in.
