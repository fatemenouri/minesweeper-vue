<template>
  <div class="container">
    <div ref="grid" class="grid"></div>
    <div class="result-section">
      <h1 :class="{ win: isWin === true, lose: gameover === true }">
        {{ result }}
      </h1>
      <button class="restart" @click.self="restart">Restart</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      gameover: false,
      isWin: false,
      width: 10,
      squares: [],
      bombs: 20,
      result: "",
      grid: "",
    };
  },
  mounted() {
    this.startGame();
  },
  methods: {
    restart() {
      this.$emit("reload");
    },
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
      this.grid = document.querySelector(".grid");
      let bombsArray = Array(this.bombs).fill("bomb");
      let dataArray = Array(this.width * this.width - this.bombs).fill("valid");
      let sumArray = dataArray.concat(bombsArray);
      let randomArray = sumArray.sort(() => Math.random() - 0.5);

      for (let i = 0; i < this.width * this.width; i++) {
        const square = document.createElement("div");
        square.setAttribute("id", i);
        square.classList.add(randomArray[i]);
        this.grid.appendChild(square);
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
          this.win();
          return;
        }

        this.checkSquare(square, currentId);
      }
      square.classList.add("checked");
      this.win();
    },

    checkSquare(square, currentId) {
      const isLeftEdge = currentId % this.width === 0;
      const isRightEdge = currentId % this.width === this.width - 1;

      setTimeout(() => {
        if (currentId > 0 && !isLeftEdge) {
          const newId = this.squares[parseInt(currentId) - 1].id;
          const newSquare = document.getElementById(newId);
          this.clicked(newSquare);
        }
        if (currentId > 9 && !isRightEdge) {
          const newId = this.squares[parseInt(currentId) + 1 - this.width].id;
          const newSquare = document.getElementById(newId);
          this.clicked(newSquare);
        }
        if (currentId > 10) {
          const newId = this.squares[parseInt(currentId - this.width)].id;
          const newSquare = document.getElementById(newId);
          this.clicked(newSquare);
        }
        if (currentId > 11 && !isLeftEdge) {
          const newId = this.squares[parseInt(currentId) - 1 - this.width].id;
          const newSquare = document.getElementById(newId);
          this.clicked(newSquare);
        }
        if (currentId < 98 && !isRightEdge) {
          const newId = this.squares[parseInt(currentId) + 1].id;
          const newSquare = document.getElementById(newId);
          this.clicked(newSquare);
        }
        if (currentId < 90 && !isLeftEdge) {
          const newId = this.squares[parseInt(currentId) - 1 + this.width].id;
          const newSquare = document.getElementById(newId);
          this.clicked(newSquare);
        }
        if (currentId < 88 && !isRightEdge) {
          const newId = this.squares[parseInt(currentId) + 1 + this.width].id;
          const newSquare = document.getElementById(newId);
          this.clicked(newSquare);
        }
        if (currentId < 89) {
          const newId = this.squares[parseInt(currentId) + this.width].id;
          const newSquare = document.getElementById(newId);
          this.clicked(newSquare);
        }
      }, 10);
    },

    gameOver(square) {
      this.gameover = true;
      this.result = "GAME OVER";

      for (let i = 0; i < this.squares.length; i++) {
        if (this.squares[i].classList.contains("bomb")) {
          this.squares[i].innerHTML = "💣";
          let restart = document.querySelector(".restart");
          restart.style.display = "flex";
        }
      }
    },

    win() {
      let count = 0;
      for (let i = 0; i < this.squares.length; i++) {
        if (this.squares[i].classList.contains("checked")) {
          count++;
        }
      }
      if (count === this.squares.length - this.bombs) {
        this.isWin = true;
        this.result = "Win";

        for (let i = 0; i < this.squares.length; i++) {
          if (this.squares[i].classList.contains("bomb")) {
            this.squares[i].innerHTML = "💣";
            let restart = document.querySelector(".restart");
            restart.style.display = "flex";
          }
        }
      }
    },
  },
};
</script>

<style>
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

/* .bomb {
  background-color: red;
} */

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
  font-size: 20px;
}

.result-section {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

button {
  justify-content: center;
  text-align: center;
  align-items: center;
  width: 100px;
  border: 5px solid;
  border-color: #f5f3eb #bab7a9 #bab7a9 #fff9db;
  box-sizing: border-box;
  background-color: #dcd6bc;
  padding: 10px 0px;
  margin-bottom: 10px;
}

.win {
  color: green;
}
.lose {
  color: darkred;
}
</style>