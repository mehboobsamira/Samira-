const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

let hearts = [];

class Heart {
  constructor() {
    this.x = Math.random() * canvas.width;
    this.y = canvas.height + Math.random() * 200;
    this.size = Math.random() * 12 + 8;
    this.speed = Math.random() * 1.5 + 0.5;
    this.alpha = Math.random();
  }

  draw() {
    ctx.save();
    ctx.globalAlpha = this.alpha;
    ctx.fillStyle = "#ff5fa2";
    ctx.beginPath();
    ctx.moveTo(this.x, this.y);
    ctx.bezierCurveTo(
      this.x - this.size,
      this.y - this.size,
      this.x - this.size * 2,
      this.y + this.size,
      this.x,
      this.y + this.size * 2
    );
    ctx.bezierCurveTo(
      this.x + this.size * 2,
      this.y + this.size,
      this.x + this.size,
      this.y - this.size,
      this.x,
      this.y
    );
    ctx.fill();
    ctx.restore();
  }

  update() {
    this.y -= this.speed;
    if (this.y < -50) {
      this.y = canvas.height + 100;
      this.x = Math.random() * canvas.width;
    }
    this.draw();
  }
}

function init() {
  hearts = [];
  for (let i = 0; i < 120; i++) {
    hearts.push(new Heart());
  }
}

function animate() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  hearts.forEach(h => h.update());
  requestAnimationFrame(animate);
}

window.addEventListener("resize", () => {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
  init();
});

init();
animate();
