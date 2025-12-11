[Uploading index.html…]()
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<title>快来打我呀~ 小地鼠游戏</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
    body {
        margin: 0; padding: 0; background: linear-gradient(to bottom, #a8e6cf, #dcedc1);
        font-family: "Comic Sans MS", "微软雅黑", sans-serif;
        text-align: center; overflow: hidden;
    }
    h1 { color: #ff6b6b; margin: 20px 0; font-size: 3em; text-shadow: 3px 3px 0px #fff; }
    .info { font-size: 1.5em; margin: 10px; color: #333; }
    #score { color: #e91e63; font-weight: bold; }
    #time { color: #4caf50; font-weight: bold; }
    .game {
        width: 600px; height: 400px; margin: 20px auto;
        background: #8bc34a; border-radius: 20px;
        display: grid; grid-template-columns: repeat(3, 1fr);
        gap: 20px; padding: 30px; box-sizing: border-box;
        box-shadow: 0 10px 30px rgba(0,0,0,0.3);
    }
    .hole {
        background: #5d4037; border-radius: 50%;
        position: relative; overflow: hidden;
        cursor: pointer;
    }
    .mole {
        width: 80%; height: 80%; background: #ff9800 url('https://s2.loli.net/2024/04/12/9K5mOaZc8tYJqBh.png') center/cover;
        position: absolute; bottom: -80%; left: 10%;
        transition: all 0.3s;
    }
    .hole.up .mole { bottom: 0; }
    .start-btn {
        padding: 15px 40px; font-size: 1.5em; background: #ff5722;
        color: white; border: none; border-radius: 50px; cursor: pointer;
        box-shadow: 0 5px 15px rgba(0,0,0,0.3);
    }
    .start-btn:hover { transform: scale(1.1); }
    footer { margin-top: 30px; color: #666; }
</style>
</head>
<body>

<h1>快来打我呀~</h1>
<div class="info">分数：<span id="score">0</span>　　剩余时间：<span id="time">30</span>s</div>

<button class="start-btn" onclick="startGame()">开始游戏！</button>

<div class="game" id="gameBoard"></div>

<footer>你的个人小游戏网站已经上线啦！地址：https://你的用户名.github.io</footer>

<audio id="hitSound" src="https://assets.mixkit.co/sfx/preview/mixkit-arcade-game-jump-coin-216.mp3"></audio>
<audio id="bgm" loop src="https://assets.mixkit.co/sfx/preview/mixkit-arcade-game-level-up-2064.mp3"></audio>

<script>
let score = 0;
let timeLeft = 30;
let timerId = null;
let moleTimer = null;
const hitSound = document.getElementById('hitSound');
const bgm = document.getElementById('bgm');

function createBoard() {
    const gameBoard = document.getElementById('gameBoard');
    gameBoard.innerHTML = '';
    for(let i = 0; i < 9; i++) {
        const hole = document.createElement('div');
        hole.classList.add('hole');
        const mole = document.createElement('div');
        mole.classList.add('mole');
        hole.appendChild(mole);
        hole.addEventListener('click', () => {
            if(hole.classList.contains('up')) {
                score++;
                document.getElementById('score').textContent = score;
                hitSound.currentTime = 0;
                hitSound.play();
                hole.classList.remove('up');
            }
        });
        gameBoard.appendChild(hole);
    }
}

function randomMole() {
    const holes = document.querySelectorAll('.hole');
    holes.forEach(h => h.classList.remove('up'));
    if(timeLeft <= 0) return;
    const randomIndex = Math.floor(Math.random() * 9);
    holes[randomIndex].classList.add('up');
}

function countDown() {
    timeLeft--;
    document.getElementById('time').textContent = timeLeft;
    if(timeLeft <= 0) {
        clearInterval(timerId);
        clearInterval(moleTimer);
        bgm.pause();
        alert('游戏结束！你的最终得分：' + score + ' 分！');
    }
}

function startGame() {
    score = 0; timeLeft = 30;
    document.getElementById('score').textContent = '0';
    document.getElementById('time').textContent = '30';
    createBoard();
    document.querySelector('.start-btn').style.display = 'none';
    
    bgm.currentTime = 0;
    bgm.play();
    
    timerId = setInterval(countDown, 1000);
    moleTimer = setInterval(randomMole, 800);
}

createBoard(); // 页面打开先显示空洞
</script>
</body>
</html>
