<template>
  <h1 class="title">{{ title }}</h1>
  <button @click="startGame()">start game</button>
  <div class="container">
    <div ref="grid" class="grid"></div>
    <div class="result-section">
      <h1 class="result"></h1>
      <button class="restart">restart</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      title: "MINE SWEEPER",
      gameover: false,
      width: 10,
      squares: [],
    };
  },

  beforeMount() {
    this.startGame();
  },
  methods: {
    startGame() {
      this.createGrid();
      for (let i = 0; i < 100; i++) {
        const leftSection = i % this.width === 0;
        const rightSection = i % this.width === this.width - 1;
        let data = 0;
        if (this.squares[i].classList.contains("valid")) {
          if (
            i > 0 &&
            !leftSection &&
            this.squares[i - 1].classList.contains("bomb")
          )
            data++;
          if (
            i < 98 &&
            !rightSection &&
            this.squares[i + 1].classList.contains("bomb")
          )
            data++;
          if (
            i < 90 &&
            !leftSection &&
            this.squares[i - 1 + this.width].classList.contains("bomb")
          )
            data++;
          if (i < 89 && this.squares[i + this.width].classList.contains("bomb"))
            data++;
          if (
            i < 88 &&
            !rightSection &&
            this.squares[i + this.width + 1].classList.contains("bomb")
          )
            data++;
          if (
            i > 11 &&
            !leftSection &&
            this.squares[i - this.width - 1].classList.contains("bomb")
          )
            data++;
          if (i > 10 && this.squares[i - this.width].classList.contains("bomb"))
            data++;
          if (
            i > 9 &&
            !rightSection &&
            this.squares[i + 1 - this.width].classList.contains("bomb")
          )
            data++;
          this.squares[i].setAttribute("data", data);
        }
      }
    },
    createGrid() {
      let grid = document.querySelector(".grid");
      let bombs = 20;

      let bombsArray = Array(bombs).fill("bomb");
      let dataArray = Array(this.width * this.width - bombs).fill("valid");
      let sumArray = dataArray.concat(bombsArray);
      let randomArray = sumArray.sort(() => Math.random() - 0.5);

      for (let i = 0; i < this.width * this.width; i++) {
        const square = document.createElement("div");
        square.setAttribute("id", i);
        square.classList.add(randomArray[i]);
        grid.appendChild(square);
        this.squares.push(square);
        square.addEventListener("click", () => {
          this.clicked(square);
        });
      }
    },

    clicked(square) {
      if (this.gameover) return;
      let currentId = square.id;
      let data = square.getAttribute("data");
      if (square.classList.contains("checked")) return;
      if (square.classList.contains("bomb")) {
        this.gameOver(square);
      } else {
        if (data != 0) {
          square.innerHTML = data;
          square.classList.add("checked");
          return;
        }

        this.checkSquare(square, currentId);
      }
      square.classList.add("checked");
    },

    checkSquare(square, currentId) {
      const isLeftEdge = currentId % this.width === 0;
      const isRightEdge = currentId % this.width === this.width - 1;

      setTimeout(() => {
        if (currentId > 0 && !isLeftEdge) {
          const newId = squares[parseInt(currentId) - 1].id;
          const newSquare = document.getElementById(newId);
          clicked(newSquare);
        }
        if (currentId > 9 && !isRightEdge) {
          const newId = squares[parseInt(currentId) + 1 - width].id;
          const newSquare = document.getElementById(newId);
          clicked(newSquare);
        }
        if (currentId > 10) {
          const newId = squares[parseInt(currentId - width)].id;
          const newSquare = document.getElementById(newId);
          clicked(newSquare);
        }
        if (currentId > 11 && !isLeftEdge) {
          const newId = squares[parseInt(currentId) - 1 - width].id;
          const newSquare = document.getElementById(newId);
          clicked(newSquare);
        }
        if (currentId < 98 && !isRightEdge) {
          const newId = squares[parseInt(currentId) + 1].id;
          const newSquare = document.getElementById(newId);
          clicked(newSquare);
        }
        if (currentId < 90 && !isLeftEdge) {
          const newId = squares[parseInt(currentId) - 1 + width].id;
          const newSquare = document.getElementById(newId);
          clicked(newSquare);
        }
        if (currentId < 88 && !isRightEdge) {
          const newId = squares[parseInt(currentId) + 1 + width].id;
          const newSquare = document.getElementById(newId);
          clicked(newSquare);
        }
        if (currentId < 89) {
          const newId = squares[parseInt(currentId) + width].id;
          const newSquare = document.getElementById(newId);
          clicked(newSquare);
        }
      }, 10);
    },

    gameOver(square) {
      this.gameover = true;
      let result = document.querySelector(".result");
      result.innerHTML = "GAME OVER";

      for (let i = 0; i < this.squares.length; i++) {
        if (this.squares[i].classList.contains("bomb")) {
          this.squares[i].innerHTML = "💣";
          let restart = document.querySelector(".restart");
          restart.style.display = "flex";
          restart.addEventListener("click", () => {
            // gameover=false
            // grid.innerHTML="";
            // bombsArray=Array(bombs).fill('bomb');
            // dataArray=Array(width*width-bombs).fill('valid');
            // sumArray=dataArray.concat(bombsArray);
            // randomArray=sumArray.sort(()=>Math.random() -0.5);
            // createGrid()
            location.reload();
          });
        }
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.title {
  color: #2c3e50;
}

.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-family: "Barlow Semi Condensed", sans-serif;
}

.grid {
  display: flex;
  height: 500px;
  width: 500px;
  flex-wrap: wrap;
  background-color: #dcd6bc;
  border: 10px solid #dcd6bc;
}

.grid div {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 50px;
  width: 50px;
  border: 5px solid;
  border-color: #f5f3eb #bab7a9 #bab7a9 #fff9db;
  box-sizing: border-box;
}

.bomb {
  background-color: red;
}

.checked {
  background-color: #c6bcbc;
  font-size: 30px;
}

.restart {
  justify-content: center;
  text-align: center;
  align-items: center;
  display: none;
  width: 100px;
}

.result-section {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
