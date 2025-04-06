# 🐍 Snake Oyunu

HTML5 Canvas ve Vanilla JavaScript ile geliştirilmiş klasik Snake oyunu.

[![Play Online](https://img.shields.io/badge/Play-GitHub%20Pages-blue)](https://rezoD51.github.io/SnakeGame/)
## 🎮 Oynanış ve Kurallar
- **Amaç**: Yılanı kontrol ederek elmaları toplayıp skoru artırmak
- **Kontroller**:
  - ←↑→↓ : Yön tuşları ile hareket
  - SPACE : Oyun bitiminde yeniden başlatma
- **Kurallar**:
  - Duvarlara veya kendi kuyruğuna çarpmadan elmaları topla
  - Her elma +10 puan
  - En yüksek skor lokal depolamada saklanır

## 📸 Ekran Görüntüleri

| Başlangıç Ekranı |   

|![Start Screen](Snake-Oyunu/assets/screenshots/start.png) |

| Oyun İçi |

|![Gameplay](Snake-Oyunu/assets/screenshots/gameplay.png) |

| Oyun Sonu | 

|![Gameend](Snake-Oyunu/assets/screenshots/gameend.png) |

## ⚙️ Teknolojiler
- HTML5 Canvas
- Vanilla JavaScript
- CSS3
- Google Fonts (Press Start 2P)

## 📁 Kod Yapısı
```javascript
class SnakeGame {
  constructor() {
    // Oyun durumu ve DOM elementleri
    this.init()       // Oyunu başlat
    this.reset()      // Oyunu sıfırla
    this.spawnApple() // Yeni elma oluştur
    this.endGame()    // Oyun sonu işlemleri
  }
  
  // Ana oyun döngüsü
  loop() {
    this.update() // Konum ve çarpışma kontrolü
    this.draw()   // Tüm görsel elementlerin çizimi

1. HTML Yapısı
----------------------------------------------------------------------------------------------------------
<div class="game-container">
  <div class="score-panel">
    <div>SKOR: <span id="score">0</span></div>
    <div>EN YÜKSEK SKOR: <span id="high-score">0</span></div>
  </div>
  <canvas id="game" width="700" height="600"></canvas>
</div>

// Skor Paneli: Mevcut skor ve en yüksek skor bilgilerini gösterir.
// Canvas: Oyunun çizildiği 700x600 piksel boyutunda bir alan.
----------------------------------------------------------------------------------------------------------
2. JavaScript Yapısı
A. SnakeGame Sınıfı
// Oyunun tüm mantığını yöneten temel sınıf.
----------------------------------------------------------------------------------------------------------
B. Constructor (Yapıcı Metod)

constructor() {
  this.isProcessingInput = false;
  this.canvas = document.getElementById('game');
  this.context = this.canvas.getContext('2d');
  this.scoreElement = document.getElementById('score');
  this.highScoreElement = document.getElementById('high-score');
  document.addEventListener('keydown', this.onKeyPress.bind(this));
  
  // Oyun durumu değişkenleri
  this.gameStarted = false;
  this.gameOver = false;
  this.score = 0;
  this.highScore = localStorage.getItem('snakeHighScore') || 0;
  this.highScoreElement.textContent = this.highScore;
}

// Değişkenler: Oyun durumu, skor, canvas bağlantıları.
// Event Listener: Klavye girişlerini dinler.
----------------------------------------------------------------------------------------------------------
C. init() Metodu

init() {
  this.positionX = this.positionY = 15; // Yılanın başlangıç pozisyonu
  this.appleX = this.appleY = 5;        // Elmanın başlangıç pozisyonu
  this.tailSize = 5;                    // Başlangıçta 5 parçalık kuyruk
  this.trail = [];                      // Yılanın kuyruk pozisyonları
  this.gridSize = 20;                   // Kare boyutu (piksel)
  this.tileCount = 30;                  // Izgara boyutu (kare sayısı)
  this.velocityX = this.velocityY = 0;  // Hareket yönü
  this.gameOver = false;
  this.score = 0;
  
  // Renk paleti ve zamanlayıcı
  this.timer = setInterval(this.loop.bind(this), 1000 / 15); // ~15 FPS
}

// Başlangıç Değerleri: Yılan ve elma pozisyonları, hız, skor sıfırlama.
// Zamanlayıcı: Oyun döngüsünü başlatır.
----------------------------------------------------------------------------------------------------------
D. Oyun Döngüsü (loop(), update(), draw())

loop() {
  if (this.gameStarted && !this.gameOver) {
    this.update(); // Konum ve çarpışma kontrolü
  }
  this.draw();     // Görsel çizimler
}

update() {
  // Yılanın hareketi
  this.positionX += this.velocityX;
  this.positionY += this.velocityY;

  // Duvar çarpışma kontrolü
  if (this.positionX < 0 || this.positionX >= this.tileCount || 
      this.positionY < 0 || this.positionY >= this.tileCount) {
    this.endGame();
  }

  // Kendine çarpma kontrolü
  this.trail.forEach(t => {
    if (t.positionX === this.positionX && t.positionY === this.positionY) {
      this.endGame();
    }
  });

  // Kuyruk güncelleme
  this.trail.push({ positionX: this.positionX, positionY: this.positionY });
  while (this.trail.length > this.tailSize) {
    this.trail.shift();
  }

  // Elma toplama
  if (this.positionX === this.appleX && this.positionY === this.appleY) {
    this.tailSize++;
    this.score += 10;
    this.spawnApple(); // Yeni elma oluştur
  }
}

draw() {
  // Arkaplan ve ızgara çizimi
  this.context.fillStyle = this.colors.background;
  this.context.fillRect(0, 0, this.canvas.width, this.canvas.height);

  // Yılan çizimi
  this.trail.forEach((t, index) => {
    const isHead = index === this.trail.length - 1;
    this.context.fillStyle = isHead ? this.colors.snakeHead : this.colors.snake;
    this.context.roundRect(...); // Yuvarlak dikdörtgen çiz
  });

  // Elma çizimi (pulse efekti ile)
  const appleSize = (this.gridSize - 5) * (1 + this.applePulse);
  this.context.arc(...); // Elma çiz
}

// update(): Yılanın hareketi, çarpışma kontrolü, skor artışı.
// draw(): Canvas üzerine yılan, elma ve arkaplanın çizimi.
----------------------------------------------------------------------------------------------------------
E. Yardımcı Metodlar

spawnApple() {
  // Elmayı rastgele konuma yerleştir (yılanın üzerine gelmeyecek şekilde)
  let validPosition = false;
  while (!validPosition) {
    this.appleX = Math.floor(Math.random() * this.tileCount);
    this.appleY = Math.floor(Math.random() * this.tileCount);
    validPosition = !this.trail.some(t => t.positionX === this.appleX && t.positionY === this.appleY);
  }
}

endGame() {
  this.gameOver = true;
  clearInterval(this.timer); // Zamanlayıcıyı durdur
  // En yüksek skoru güncelle
  if (this.score > this.highScore) {
    localStorage.setItem('snakeHighScore', this.score);
  }
}
----------------------------------------------------------------------------------------------------------
F. Kullanıcı Girişi (onKeyPress())

onKeyPress(e) {
  // SPACE tuşu: Yeniden başlat
  if (this.gameOver && e.keyCode === 32) {
    this.reset();
    return;
  }

  // Yön tuşları: Hareket başlat
  if (!this.gameStarted && (e.keyCode >= 37 && e.keyCode <= 40)) {
    this.gameStarted = true;
    switch(e.keyCode) {
      case 37: this.velocityX = -1; break; // Sol
      case 38: this.velocityY = -1; break; // Yukarı
      case 39: this.velocityX = 1;  break; // Sağ
      case 40: this.velocityY = 1;  break; // Aşağı
    }
  }

  // Hareket sırasında yön değiştirme
  if (!this.isProcessingInput) {
    // Örneğin: Sağa giderken sola dönemez
    if (e.keyCode === 37 && this.velocityX !== 1) { /*...*/ }
  }
}

// Yön Tuşları: Yılanın hareketini başlatır.
// SPACE Tuşu: Oyunu yeniden başlatır.
// Engelleme Mekanizması: Ardışık tuş basımlarını önler.
----------------------------------------------------------------------------------------------------------
3. CSS Yapısı

// Responsive Tasarım: Skor paneli ve canvas sabit genişlikte.
// Retro Stil: Press Start 2P fontu ve koyu renk teması.

----------------------------------------------------------------------------------------------------------
4. Önemli Özellikler

// Lokal Depolama: En yüksek skor localStorage ile saklanır.
// Pulse Efekti: Elma büyüyüp küçülerek dikkat çeker.
// Göz Çizimi: Yılanın başı yönüne göre gözler konumlandırılır.
// Yuvarlak Köşeler: Yılan ve elma görselleri yuvarlak köşelidir.

----------------------------------------------------------------------------------------------------------
5. Oyun Akışı

// Başlangıç Ekranı: "Bir yön tuşuna basın" mesajı.
// Oyun İçi: Yılanı yön tuşlarıyla kontrol et, elmaları topla.
// Oyun Sonu: Duvara veya kuyruğa çarpınca skor gösterilir.
// Yeniden Başlatma: SPACE tuşuna basarak tekrar oyna.
  }
}
