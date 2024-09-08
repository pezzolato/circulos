<!DOCTYPE html>
<!--
Programa: circulos em movimento
Nome: Andressa Pezzolato de Almeida - Nº: 02 - Turma: 2B
Nome: Gabrielle Santos Mendes Léo - Nº: 10.
Nome: Gustavo Batista Santos - Nº: 13.
DESCRIÇÃO: Há dois círculos, um amarelo e um azul, onde eles estão em movimento e se cruzam na tela.
-->
<html lang="pt-BR">
<head>
  <title>Circulos se movendo</title>
  <meta charset="UTF-8">
  <style>
      canvas {
          border: 1px solid #d3d3d3;
          background-color: #A0A0A0;
      }
  </style>
  <script>
      var yellowCircle, blueCircle;

      function startGame() {
          yellowCircle = new component(20, "yellow", 50, 50, 5, 5);  // Círculo amarelo começa em (50, 50)
          blueCircle = new component(20, "blue", 450, 450, -5, -5);  // Círculo azul começa em (450, 450)
          myGameArea.start();
      }

      var myGameArea = {
          canvas: document.createElement("canvas"),
          start: function() {
              this.canvas.width = 500;
              this.canvas.height = 500;
              this.context = this.canvas.getContext("2d");
              document.body.insertBefore(this.canvas, document.body.childNodes[0]);
              this.interval = setInterval(updateGameArea, 20);
          },
          clear: function() {
              this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
          }
      }

      function component(radius, color, x, y, dx, dy) {
          this.radius = radius;
          this.color = color;
          this.x = x;
          this.y = y;
          this.dx = dx;
          this.dy = dy;

          this.update = function() {
              var ctx = myGameArea.context;
              ctx.beginPath();
              ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
              ctx.fillStyle = this.color;
              ctx.fill();
              ctx.closePath();
          }

          this.newPos = function() {
              this.x += this.dx;
              this.y += this.dy;

              // Verifica se o círculo amarelo chegou ao lado oposto
              if (this.dx > 0 && (this.x >= 450 || this.y >= 450)) {
                  this.dx = 0;
                  this.dy = 0;
              }
              // Verifica se o círculo azul chegou ao lado oposto
              if (this.dx < 0 && (this.x <= 50 || this.y <= 50)) {
                  this.dx = 0;
                  this.dy = 0;
              }
          }
      }

      function updateGameArea() {
          myGameArea.clear();
          yellowCircle.newPos();
          blueCircle.newPos();
          yellowCircle.update();
          blueCircle.update();

          // Para a animação quando ambos os círculos pararem
          if (yellowCircle.dx === 0 && blueCircle.dx === 0) {
              clearInterval(myGameArea.interval);
          }
      }
  </script>
</head>
<body onload="startGame();">
</body>
</html>
