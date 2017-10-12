# fengshu
绯炎枫叶的枫树
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Myself_Game_01</title>
	<style media='screen'>
		canvas{
			border: 1px black solid;
		}
	</style>
</head>
<body>
	<canvas id ="id-canvas" width="400" height="300"></canvas>
	<script>
		var log = console.log.bind(console)

		var imageFormPath = function (path) {
			var img = new Image()
			img.src = path
			return img
		}

		var	Foot = function() {
			var image = imageFormPath('foot.png')
			var o = {
				image: image,
				x: 100,
				y: 250,
				speed: 10,
			}

			o.moveLeft = function() {
				// body...
				o.x -= o.speed
			}
			o.moveRight = function() {
				// body...
				o.x += o.speed
			}
			o.colide = function(ball) {
				if (ball.y + ball.image.height > o.y) {
					if (ball.x > o.x && ball.x < o.x + o.image.width) {
						return true	
					}
				} else {
					return false	
				}
				
			}
			return o
		}

		var	Ball = function() {
			var image = imageFormPath('ball.png')
			var o = {
				image: image,
				x: 150,
				y: 150,
				speedx: 7,
				speedy: 7,
				fired: false,
			}
			o.fire = function(){
				o.fired = true
			}
			o.move = function(){
				if (o.fired) {
					if (o.x < 0 || o.x > 400) {
						o.speedx = -o.speedx
					}
					if (o.y < 0 || o.y > 300) {
						o.speedy = -o.speedy
					}
					//move_event
					o.x += o.speedx
					o.y += o.speedy
				}
			}
			
			return o
		}

		var yaGame = function() {
			var y = {
				actions:{},
				keydowns:{},
			}

			var canvas = document.querySelector('#id-canvas')
			var context = canvas.getContext('2d')
			y.canvas = canvas
			y.context = context
			//drawImage
			y.drawImage = function(yaImg) {
				y.context.drawImage(yaImg.image,yaImg.x,yaImg.y)
			}

			//event
			window.addEventListener('keydown',function(event){
				y.keydowns[event.key] = true
			})

			window.addEventListener('keyup',function(event){
				y.keydowns[event.key] = false
			})

			//
			y.registerAction = function(key, callback) {
				y.actions[key] = callback
			}
			//time
			setInterval(function() {
				//events
				var actions = Object.keys(y.actions)
				for (var i = 0; i < actions.length; i++) {
					var key = actions[i]
					if (y.keydowns[key]) {
						//如果按键被按下，调用注册的action
						y.actions[key]() 
					}
				}
				//update
				y.update() //先注释
				//clear
				context.clearRect(0,0,canvas.width,canvas.height)
				//draw
				y.draw()
			},1000/30)

			return y
		}

		var __main = function() {
			
			var game = yaGame()
			var foot = Foot()
			var ball = Ball()

			game.registerAction('a',function() {
				foot.moveLeft()
			})

			game.registerAction('d',function() {
				foot.moveRight()
			})

			game.registerAction('f',function() {
				ball.fire()
			})

			game.update = function() {
				ball.move()
				if (foot.colide(ball)) {
					ball.speedy *= -1
				}
			}

			game.draw = function() {
				//draw
				//game.context.drawImage(foot.image,foot.x,foot.y)
				game.drawImage(foot)
				game.drawImage(ball)
			}
			
		}

		__main()
	</script>
</body>
</html>
