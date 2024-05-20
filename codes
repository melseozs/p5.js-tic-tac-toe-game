let cols = 3; 
let rows = 3; 
let size; 
let board = []; // oyun tahtasını temsil eden 2D dizi
let button; // başlat düğmesi
let players = ['X', 'O']; 
let playerScores = {
  'X': 0,
  'O': 0
}; 
let currentPlayer;
let round = 1; 
let roundInfo;
let winner = false; // kazanan olup olmadığını belirten değişken
let winnerLoc = []; // kazananın konumunu saklayan dizi
let winnerText; 
let startButton; 
let imgx; 
let imgo; 
let nimgx; 
let nimgo;
let aud; // hamle sesi
let winsound; // kazanma sesi
let tiesound; // beraberlik sesi


function preload(){ 
  // resimleri ve ses dosyalarını yükler
  imgx = loadImage('assets/imgx.png'); 
  imgo = loadImage('assets/imgo.png'); 
  nimgx = loadImage('assets/nimgx.jpg');
  nimgo = loadImage('assets/nimgo.jpg');
  aud = createAudio('assets/hsound.mpeg');
  winsound = createAudio('assets/winsound.mpeg');
  tiesound = createAudio('assets/tiesound.mpeg');
}



function setup() { 
// canvas oluşturur, butonları ve stilleri ayarlar, oyun tahtasını ve oyuncuyu başlatır
  createCanvas(600, 400);
  size = 400 / cols;
  roundInfo = createP(''); // tur bilgisi için html elemanı oluşturuluyor
  roundInfo.position(440, 20); 
  startButton = createButton('Start New Round');

  // düğme stilleri ayarlanıyor
  startButton.style('font-family', 'Ink Free, Arial, sans-serif');
  startButton.style('background', 'linear-gradient(#8A2BE2, #5E3266)');
  startButton.style('border-radius', '10px');
  startButton.style('color', 'white');
  startButton.style('font-size', '16px');
  startButton.position(430, 300);
  
  startButton.mousePressed(startNewRound); // düğmeye tıklanınca yeni tur başlatılıyor

  // düğmeye hover efekti ekleniyor
  startButton.mouseOver(() => {
    startButton.style('background', 'linear-gradient(#800080, #7F62B2)');
  });
  startButton.mouseOut(() => {
    startButton.style('background', 'linear-gradient(#8A2BE2, #D39DDD)');
  });

  currentPlayer = players[floor(random(2))]; // rastgele bir oyuncu seçiliyor
  
  initializeBoard(); // oyun tahtası başlatılıyor
}



function startNewRound() {
  round++; // tur sayısı artırılıyor
  currentPlayer = players[floor(random(2))]; // rastgele bir oyuncu seçiliyor
  initializeBoard(); // oyun tahtası başlatılıyor
  winner = false; // kazanan sıfırlanıyor
  winnerText = ""; // kazanan metni sıfırlanıyor
}

function initializeBoard() {
  // yeni tur için tahtayı sıfırlıyor
  for (let i = 0; i < cols; i++) {
    board[i] = [];
    for (let j = 0; j < rows; j++) {
      board[i][j] = 0; // tüm hücreler sıfırlanıyor
    }
  }
  updateRoundInfo(); // tur bilgisi güncelleniyor
}

function draw() {
  background('#DAB9F5'); // arka plan rengi ayarlanıyor
  drawBoard(); // oyun tahtası çiziliyor
  drawWinner(); // kazanan çiziliyor
  drawPlayerScores(); // oyuncu puanları çiziliyor
  updateRoundInfo(); // tur bilgisi güncelleniyor
}

function updateRoundInfo() {
  roundInfo.html("Round " + round + ": " + currentPlayer + "'s turn."); // tur bilgisi güncelleniyor
}

function drawBoard() {
  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      rect(i * size, j * size, size, size); // hücreler çiziliyor
      if (board[i][j] !== 0) {
        let xCoord = i * size + size / 2;
        let yCoord = j * size + size / 2;
        let imageSize = min(size * 0.8, min(imgx.width, imgo.width)); // resim boyutu ayarlanıyor 
        if (board[i][j] === 'X') {
          image(imgx, xCoord - imageSize / 2, yCoord - imageSize / 2, imageSize, imageSize); // X resmi basılıyor 
        } else {
          image(imgo, xCoord - imageSize / 2, yCoord - imageSize / 2, imageSize, imageSize); // O resmi basılıyor
        }
      }
    }
  }
}

function placePieces(x, y) {
  if (board[x] && board[x][y] === 0) {
    board[x][y] = currentPlayer; // tahta güncelleniyor
    winnerText = currentPlayer; 
    if (currentPlayer == 'X') {
      currentPlayer = 'O'; // oyuncu değiştiriliyor
    } else {
      currentPlayer = 'X';
    }
    checkWinner(); // kazanan kontrol ediliyor

    let newAud = new Audio(aud.src); 
    newAud.play(); // hamle sesi oynatılıyor
    
    setTimeout(function(){
      newAud.pause();
    }, 1600); // ses 1.6 saniye sonra durduruluyor
  } 
}

function mousePressed() {
  if (!winner && winnerText !== "TIE") { // kazanan yoksa ve oyun berabere bitmediyse hamle yapılabilir
    let index = [floor(mouseX / size), floor(mouseY / size)];
    let x = index[0];
    let y = index[1];

    if (x >= 0 && x < cols && y >= 0 && y < rows && board[x][y] === 0) {
      placePieces(x, y); // geçerli hücreye hamle yapılır
    }
    else{
      print('spot not available');
    }
  }
}


function checkWinner() {
  // satırları kontrol et. row
  for (let i = 0; i < cols; i++) {
    if (board[i][0] == board[i][1] && board[i][1] == board[i][2] && board[i][0] != 0) {
      winner = true;
      winnerLoc = [1, i];
    }
  }

  // sütunları kontrol et. cols
  for (let i = 0; i < rows; i++) {
    if (board[0][i] == board[1][i] && board[1][i] == board[2][i] && board[0][i] != 0) {
      winner = true;
      winnerLoc = [2, i];
    }
  }

  // sol çaprazı kontrol et
  if (board[0][0] == board[1][1] && board[1][1] == board[2][2] && board[0][0] != 0) {
    winner = true;
    winnerLoc = [3, 0];
  }

  // sağ çaprazı kontrol et
  if (board[2][0] == board[1][1] && board[1][1] == board[0][2] && board[2][0] != 0) {
    winner = true;
    winnerLoc = [4, 0];
  }

  // beraberliği kontrol et
  if (!winner && boardIsFull()) {
    winnerText = "TIE";
  }

  // kazanan varsa puan güncellenir ve kazanan sesi çalınır
  if (winner && winnerText !== "TIE") {
    playerScores[winnerText]+=2;
    
    let newWinSound = new Audio(winsound.src);
    newWinSound.play();
  }
  // beraberlik varsa puan güncellenir ve beraberlik sesi çalınır
  else if (winnerText === "TIE") {
    playerScores['X']++;
    playerScores['O']++;
    
    let newTiesound = new Audio(tiesound.src);
    newTiesound.play();
  }
}

function drawWinner() {
  stroke('#8A2BE2'); 
  strokeWeight(3); 
  if (winner) {
    textAlign(CENTER); 
    fill('white'); 
    textSize(50); 
    text(winnerText + " wins!", 500, 375); // kazanan yazısı
    
    if (winnerLoc[0] == 1) {
      line(size / 2 + winnerLoc[1] * size, size / 2, size / 2 + winnerLoc[1] * size, size / 2 + 2 * size); // dikey çizgi
    } else if (winnerLoc[0] == 2) {
      line(size / 2, winnerLoc[1] * size + size / 2, size / 2 + 2 * size, size / 2 + winnerLoc[1] * size); // yatay çizgi
    } else if (winnerLoc[0] == 3) {
      line(size / 2, size / 2, size / 2 + 2 * size, size / 2 + 2 * size); // sol çapraz çizgi
    } else if (winnerLoc[0] == 4) {
      line(size / 2 + 2 * size, size / 2, size / 2, size / 2 + 2 * size); // sağ çapraz çizgi
    }
  } else if (winnerText === "TIE") {
    textAlign(CENTER);
    fill('white');
    textSize(50);
    text("TIE!", 500, 375); 
  }
}

function drawPlayerScores() {
  textSize(20); 
  textAlign(LEFT); 
  image(nimgx, 425, 95, 75, 75); 
  image(nimgo, 425, 205, 75, 75); 
  textSize(50); 
  fill('white');
  text(playerScores['X'], 545, 150);
  text(playerScores['O'], 545, 260);
}

function boardIsFull() {
  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      if (board[i][j] === 0) {
        return false; // boş hücre varsa false döndür
      }
    }
  }
  return true; // tüm hücreler doluysa true döndür
}
