- Enables drawing graphics via JavaScript and the HTML `<canvas>` element
```
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');
```

### ==example== Create a rectangle
- this code will produce a `200px` by `80px` red rectangle with 50% opacity.
```
ctx.fillStyle = "rgba(255, 0, 0, 0.5)";
ctx.fillRect(0, 0, 200, 80);
```

### ==example== Creating different shapes
- this code will create a square and then cut out the top 1/4th of the square
```
ctx.fillRect(50, 50, 200, 200);
ctx.clearRect(50, 50, 100, 100);
```

### ==example== Create outline of a shape
- this code creates the outline of a square with purple borders and a border with of `4px`
```
ctx.strokeStyle = "purple";
ctx.lineWidth = "4";
ctx.strokeRect(50, 50, 100, 100);
```

### ==example== Create a path (line)
- uses a coordinated based system, this code produces a diagonal line
```
ctx.beginPath();
ctx.moveTo(50, 50);
ctx.lineTo(100, 100);
ctx.stroke();
```

### ==example== Create a filled shape with paths
- this code produces a triangle based on 3 coordinate points
```
ctx.beginPath();
ctx.moveTo(50, 50);
ctx.lineTo(100, 100);
ctx.lineTo(200, 80);
ctx.fill();
ctx.stroke();
```

### ==example== Drawing an arc
- the arc method takes in angles in radians, this code produces a full circle
```
ctx.beginPath();
ctx.arc(145, 145, 50, 0, 2 * Math.PI);
ctx.fill();
ctx.stroke();
```

### Create a reference variable for a shape
- with the `new Path2D()` constructor we can create a reference to our shape.
```
ctx.fillStyle = "rgba(0, 255, 0, 0.5)";
const bigRectangle = new Path2D();
bigRectangle.rect(0, 0, 200, 80);
ctx.fill(bigRectangle);
ctx.stroke(bigRectangle);
```

# Create Bouncing Balls on a Canvas
```
const canvas = document.querySelector("#ballCanvas");
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

const ctx = canvas.getContext("2d");
const balls = [];

class Ball {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.xVel = (Math.random() - 0.5) * 10;
    this.yVel = (Math.random() - 0.5) * 10;
    this.size = Math.random() * 30 + 10;
    this.color = Ball.getRandomColor();
  }

  static getRandomColor() {
    const r = Math.floor(Math.random() * 256);
    const g = Math.floor(Math.random() * 256);
    const b = Math.floor(Math.random() * 256);

    return `rgb(${r}, ${g}, ${b})`;
  }

  draw() {
    ctx.beginPath();
    ctx.fillStyle = this.color;
    ctx.arc(this.x, this.y, this.size, 0, 2 * Math.PI);
    ctx.fill();
  }

  update() {
	// prevent out of bounds in x-direction
	if (this.x + this.size > canvas.width || this.x - this.size <= 0) {
	  this.xVel = -this.xVel;
	}

	// prevent out of bounds in y-direction
	if (this.y + this.size > canvas.height || this.y - this.size <= 0) {
		this.yVel = -this.yVel;
	}

    this.x += this.xVel;
    this.y += this.yVel;
   
   // gravity effect
   if (this.y + this.size < canvas.height) {
      this.yVel += 0.3;
    }
  }
}

function loop() {
  // clear the canvas before redrawing all the balls
  ctx.fillStyle = "#f2f2f2";
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  balls.forEach((ball) => {
    ball.update();
    ball.draw();
  });
  requestAnimationFrame(loop);
}
loop();

canvas.addEventListener("click", (e) => {
  const ball = new Ball(e.clientX, e.clientY);
  balls.push(ball);
});
```

