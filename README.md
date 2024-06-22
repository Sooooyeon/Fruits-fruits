# 🍉🍊Fruits_Fruits🍒🍐
![image](https://github.com/Sooooyeon/Fruits-fruits/assets/118328426/a30071a9-8e38-46dc-a70d-6a9789b981a9)



</br>

## Fruits_Fruits <img src="https://img.shields.io/badge/html5-E34F26?style=for-the-badge&logo=html5&logoColor=white"/> <img src="https://img.shields.io/badge/css3-1572B6?style=for-the-badge&logo=css3&logoColor=white"/> <img src="https://img.shields.io/badge/javascript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black">
Fruits_Fruits는 HTML5 canvas와 javascript를 사용해 제작한 웹 미니게임 입니다.
### 🚀 배포 URL : https://sooooyeon.github.io/Fruits-fruits/
### 게임 배경
> <p>한적한 마을 근처의 신비한 숲에서, 귀여운 고양이 미요가 신비한 이야기를 듣게 됩니다.
> 숲 속에는 특별한 힘을 주는 다섯 가지 과일이 있다는 것입니다.
> 호기심 가득한 미요는 이 과일을 찾아 모험을 떠나기로 결심합니다.
> 하지만 숲은 그리 호락호락하지 않습니다. 과일을 찾는 도중, 고양이를 방해하려는 바위들이 계속해서 떨어집니다.
> 미요는 과일을 모으고 안전하게 집으로 돌아가야 합니다.</p>
### 게임 목표
> <p>플레이어는 미요를 조종하여 떨어지는 과일을 모으고 바위를 피해야 합니다.
> 각 과일을 성공적으로 수집할 때마다 3점씩 점수를 얻습니다.
> 바위에 맞으면 게임이 즉시 종료됩니다.
> 100점을 획득하면 미요는 무사히 집으로 돌아갈 수 있습니다.</p>


</br>

## ✨ 게임 실행
|**화면**|**설명**|
| :-------------------------------: | :-------------------------------: |
| ![gameover](https://github.com/Sooooyeon/Fruits-fruits/assets/118328426/22d44f52-d3b6-41b5-8112-0da9ed90dcee) | enter키를 눌러 게임 실행이 가능하며, 방향키 ←, → 로 고양이를 조종합니다. 바위와 충돌 시 게임은 바로 종료됩니다. |
| ![winner](https://github.com/Sooooyeon/Fruits-fruits/assets/118328426/9433877d-7070-44dd-aaae-a9f09a27e4dd) | 한 개의 과일을 획득할 때 마다 3점의 점수를 얻으며 100점을 획득할 경우 게임이 종료됩니다. |


</br>

## 🎮 게임 구현 
### 1. 시작화면 구성 및 게임 실행화면 이동
```js
// 캔버스 객체
var threadSpeed = 16, canvas, ctx, canvasBuffer, bufferCtx;

// 게임 실행 화면 배경
var background;

// 고양이
var cat, sx, sy, sw = 100, sh = 138;

// 게임의 상태
var STATE_START = false;
var STATE_GAMEOVER = false;
var STATE_WIN = false;

// 키 상태 (게임시작 : enter, 게임다시 시작 : space, 방향키)
var keyPressed = [];

// 반복 루틴
var loopInstance;

// 점수
var score = 0;

window.addEventListener('load', initialize, false);
window.addEventListener('keydown', getKeyDown, false);
window.addEventListener('keyup', getKeyUp, false);

function initialize() {
  canvas = document.getElementById('canvas');
  if (canvas == null || canvas.getContext == null) return;

  ctx = canvas.getContext('2d');
  canvasBuffer = document.createElement('canvas');
  canvasBuffer.width = canvas.width;
  canvasBuffer.height = canvas.height;
	bufferCtx = canvasBuffer.getContext('2d');

  // 게임 시작 메시지
  startMessage();

  // 이미지 설정
  setImage();

  loopInstance = setInterval(update, threadSpeed);

}

function setImage() {
  cat = new Image();
  cat.src = "./source/cat.png";
  background = new Image();
  background.src = "./source/bg.png";
}

function startMessage() {
  drawText("bold 100px Bagel Fat One", "#313B95", "center", "FruitsFruits", 20, -60);
  drawText("bold 60px Do Hyeon", "#000000", "center", "Enter To Start", 0, 20);
  drawText("bold 30px Do Hyeon", "#000000", "center", "방향키로 조작하며, 바위와 충돌 시 게임이 종료돼요!", 0, 70);
  drawText("bold 30px Do Hyeon", "#000000", "center", "100점 이상 획득시 승리!", 0, 110);
}

function update() {
  handleKeyPress();

  if (STATE_START) {
    drawGame(); // 게임화면을 그려냄
  } 
}

function handleKeyPress() {
  if ((keyPressed[13] === true) && !STATE_START && !STATE_GAMEOVER) {
    startGame();
  }
}

function startGame() {
  STATE_START = true;
  // 캐릭터 초기 위치
  sx = canvas.width / 2 - 95;
  sy = canvas.height / 2 + 160;

  // 배경음악 재생
  var bgm = document.getElementById('bgm');
  bgm.play();
  bgm.volume = 0.3;
}


function getKeyDown(event) {
  keyPressed[event.keyCode] = true;
}

function getKeyUp(event) {
  keyPressed[event.keyCode] = false;
}
```
```css
body {
  background-color: rgb(156, 236, 80);
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0;
  height: 100vh;
  overflow: hidden;

}

canvas {
  background-image: url(./source/bg.png);
  background-size: cover;
  height: 100vh;
}
```


### 2. 고양이 이동범위 및 속도 설정
```js
function update() {
  handleKeyPress();

  if (STATE_START) {
    updatePlayerPosition(); // 고양이 위치 업데이트
    
    ...(생략)

  }
}

function updatePlayerPosition() {
  if (keyPressed[37]) { sx -= 12; } // 왼쪽으로 이동 
  if (keyPressed[39]) { sx += 12; } // 오른쪽으로 이동

  // 고양이가 캔버스 왼쪽 경계를 넘지 않도록 함.
  if (sx < 0) {
    sx = 0;
  }

  // 고양이가 캔버스 오른쪽 경계를 넘지 않도록 함.
  if (sx + sw > canvas.width) {
    sx = canvas.width - sw;
  }
}
```

### 3. 과일 생성 및 과일 이동 설정
```js
// 과일
var fruits = new Array();
var fruitsName = ['orange', 'peach', 'watermelon', 'pear', 'cherry']
var fruitsSize = 54;

function update() {
  handleKeyPress();

  if (STATE_START) {
      ...(생략)
    updateAndDrawFruit(); // 과일 출력 함수 호출

  }
}

function createFruits() {
  // 과일 생성 간격을 랜덤으로 설정
  var interval = Math.floor(Math.random() * 100) + 1000;

  // 과일 객체 생성
  setTimeout(function () {
    var fruitIndex = Math.floor(Math.random() * fruitsName.length); 
    var fruit = {
      name: fruitsName[fruitIndex], // 과일 이름
      x: Math.floor(Math.random() * (canvas.width - fruitsSize)), // 랜덤한 x 위치
      y: -fruitsSize, // 화면 상단에서 시작
      speed: Math.random() * 5 + 5 
    };

    fruits.push(fruit); // 생성된 과일을 배열에 추가

    createFruits(); // 재귀적으로 호출하여 계속해서 과일 생성
  }, interval);
}

function updateAndDrawFruit() {
	fruits.forEach(function (fruit, index) {
	  fruit.y += fruit.speed;
	  drawImage(`./source/${fruit.name}.png`, fruit.x, fruit.y, fruitsSize);
	
	  if (fruit.y > canvas.height) { // 바닥으로 떨어질 경우 제거
	    fruits.splice(index, 1);
	  }
	})
}

function drawImage(src, x, y, width, height = width) {
  let image = new Image;
  image.src = src;
  ctx.drawImage(image, x, y, width, height);
}
```



### 4. 바위 생성 및 바위 이동 설정
```js
// 바위
var rocks = new Array();
var rockSpeed = 5; // 초기 바위 속도
var rockSpeedIncrement = 0.5; // 바위 속도 증가량

function createRock() {
  var interval = Math.floor(Math.random() * 100) + 1000;

  setTimeout(function () {
    var rock = {
      x: Math.floor(Math.random() * (canvas.width - fruitsSize)), // 랜덤한 x 위치
      y: -fruitsSize, // 화면 상단에서 시작
      speed: rockSpeed
    };

    rocks.push(rock); // 생성된 장애물을 배열에 추가
    rockSpeed += rockSpeedIncrement;

    createRock(); // 재귀적으로 호출하여 계속해서 장애물 생성
  }, interval);
}

function updateAndDrawRock() {
	rocks.forEach(function (rock, index) {
	  rock.y += rock.speed;
	  drawImage(`./source/rock.png`, rock.x, rock.y, 93, 66);
	
	  if (rock.y > canvas.height) {
	    rocks.splice(index, 1);
	  }
	}
}


function drawImage(src, x, y, width, height = width) {
  let image = new Image;
  image.src = src;
  ctx.drawImage(image, x, y, width, height);
}
```

### 5. 고양이와 과일 충돌 시 점수 추가
```js
function checkCollision(item) {
  return sx < item.x + fruitsSize &&
    sx + sw > item.x &&
    sy < item.y + fruitsSize &&
    sy + sh > item.y;
}

function updateAndDrawFruit(fruit, index) {
  
  ... (생략)

  if (checkCollision(fruit)) {
    var getSound = document.getElementById('get')
    getSound.play();
    getSound.volume = 0.3;
    fruits.splice(index, 1);
    score += 3;
  }
}
```

### 6. 고양이와 바위 충돌 시 게임 종료
```js
function updateAndDrawRock(rock, index) {

	... (생략)

  if (checkCollision(rock)) {
    endGame();
  }
}

function endGame() {
  document.getElementById('gameover').play();
  document.getElementById('bgm').pause();
  STATE_START = false;
  STATE_GAMEOVER = true;
}

function update() {
  handleKeyPress();

  if (STATE_START) {
	   ...(생략)
  } else if (STATE_GAMEOVER) { 
    drawText("bold 100px arial", "#313B95", "center", "Game Over", 0, -30);
    drawText("bold 50px arial", "#000000", "center", "Spacebar to Restart", 0, 30);
  }
}
```

### 7. 100점 달성 시 승리
```js
function updateAndDrawRock(rock, index) {

	... (생략)

  if (checkCollision(rock)) {
    endGame();
	}
  if (score >= 100) {
    winGame()
  }
}

function winGame() {
  document.getElementById('win').play();
  document.getElementById('bgm').pause();
  STATE_START = false;
  STATE_GAMEOVER = true;
  STATE_WIN = true;
}

function update() {
  handleKeyPress();

  if (STATE_START) {
	   ...(생략)
  } else if (STATE_WIN) {
    drawText("bold 100px arial", "#313B95", "center", "You Win!", 0, -30);
    drawText("bold 50px arial", "#000000", "center", "Spacebar to Restart", 0, 30);
  } else if (STATE_GAMEOVER) {
    drawText("bold 100px arial", "#313B95", "center", "Game Over", 0, -30);
    drawText("bold 50px arial", "#000000", "center", "Spacebar to Restart", 0, 30);
  }
}
```

### 8. 게임 재시작
```js
function handleKeyPress() {
  if ((keyPressed[13] === true) && !STATE_START && !STATE_GAMEOVER) {
    startGame();
	}
  if (keyPressed[32] === true) {
    document.location.reload();
    startGame();
  }
}
```


</br>


## 💡 코드 개선 
### 생성 기능을 하는 함수 통합
```js
// 과일과 바위를 생성하는 함수
function createObject(type) {
  var interval = Math.floor(Math.random() * 100) + 1000;

  setTimeout(function () {
    var object;
    if (type === "fruit") {
      var fruitIndex = Math.floor(Math.random() * fruitsName.length);
      object = {
        name: fruitsName[fruitIndex],
        x: Math.floor(Math.random() * (canvas.width - fruitsSize)),
        y: -fruitsSize,
        speed: Math.random() * 5 + 5
      };
      fruits.push(object);
    } else if (type === "rock") {
      object = {
        x: Math.floor(Math.random() * (canvas.width - fruitsSize)),
        y: -fruitsSize,
        speed: rockSpeed
      };
      rocks.push(object);
      rockSpeed += rockSpeedIncrement;
    }

    createObject(type); // 같은 타입의 객체를 계속해서 생성
  }, interval);
}

// 과일 생성
function createFruits() {
  createObject("fruit");
}

// 바위 생성
function createRock() {
  createObject("rock");
}

// 과일과 바위를 화면에 그려냄
function drawObjects() {
  fruits.forEach(function (fruit, index) {
    updateAndDrawFruit(fruit, index);
  });

  rocks.forEach(function (rock, index) {
    updateAndDrawRock(rock, index);
  });
}

function updateAndDrawFruit(fruit, index) {
  fruit.y += fruit.speed;
  drawImage(`./source/${fruit.name}.png`, fruit.x, fruit.y, fruitsSize);

  if (fruit.y > canvas.height) {
    fruits.splice(index, 1);
  }

  if (checkCollision(fruit)) {
    var getSound = document.getElementById('get')
    getSound.play();
    getSound.volume = 0.3;
    fruits.splice(index, 1);
    score += 3;
  }
}

function updateAndDrawRock(rock, index) {
  rock.y += rock.speed;
  drawImage(`./source/rock.png`, rock.x, rock.y, 93, 66);

  if (rock.y > canvas.height) {
    rocks.splice(index, 1);
  }

  if (checkCollision(rock)) {
    endGame();
  }

  if (score >= 100) {
    winGame()
  }
}

function drawImage(src, x, y, width, height = width) {
  let image = new Image;
  image.src = src;
  ctx.drawImage(image, x, y, width, height);
}
```

