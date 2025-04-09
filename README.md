# 🐍 Snake Oyunu

HTML5 Canvas ve Vanilla JavaScript ile geliştirilmiş klasik Snake oyunu.

Oynamak İçin Tıkla
[![Play Online](https://img.shields.io/badge/Play-GitHub%20Pages-blue)](https://rezoD51.github.io/SnakeGame/)
## 🎮 Oynanış ve Kurallar
- **Amaç**: Verilen görevleri yaparak odalar geçmek en en yüksek skoru elde etmek 
- **Kontroller**:
  - ←↑→↓ : Yön tuşları ile hareket
  - WASD : İle Hareket
- **Kurallar**:
  - Duvarlara veya kendi kuyruğuna çarpmadan elmaları topla
  - Tüm anahtarları toplayıp kilitlere yerleştir
  - Görevde istenen süre kadar hayatta Kal
  - Soru işaretleri rastgele ektra özellik sağlar (2x puan kazanma, 15 saniye boyunca yavaş hareket etme vs.)
## 🌟 Özellikler
- Çoklu oda sistemi
- Farklı görev modları
- Rastgele oluşturulmuş labirentler
- Güçlendirme öğeleri (power-ups)
- Duyarlı tasarım (responsive design)

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

## 📁 Kod Yapısı
1. Temel Yapı

const canvas = document.getElementById('game-board');
const ctx = canvas.getContext('2d');
Oyun alanını oluşturmak için HTML5 Canvas kullanılmıştır

ctx değişkeni ile çizim işlemleri yapılır

2. Oyun Değişkenleri

let snake = []; // Yılanın segmentlerini tutan dizi
let food = []; // Yenilebilir nesneler
let walls = []; // Engel/düvar nesneleri
let xVelocity = 0, yVelocity = 0; // Yılanın hareket yönü
let score = 0, room = 1, gameTime = 0; // Oyun istatistikleri
Tüm oyun durumu bu değişkenlerde saklanır

Diziler nesnelerin konumlarını (x,y) tutar

3. Oyun Döngüsü

function gameLoop() {
    clearBoard();
    moveSnake();
    checkCollisions();
    checkMission();
    drawGame();
    updateTime();
}
Her frame'de sırasıyla:

Ekran temizlenir

Yılan hareket ettirilir

Çarpışmalar kontrol edilir

Görev durumu kontrol edilir

Oyun çizilir

Zaman güncellenir

4. Yılan Hareketi

function moveSnake() {
    const head = {
        x: (snake[0].x + xVelocity + tileCount) % tileCount,
        y: (snake[0].y + yVelocity + tileCount) % tileCount
    };
    snake.unshift(head);
    if (!checkFoodCollision(head)) snake.pop();
}
Yılanın başına yeni bir segment eklenir

Yemek yenmediyse kuyruktan bir segment çıkarılır

Modulo operatörü ile ekran sınırlarında dönme sağlanır

5. Çarpışma Kontrolü

function checkCollisions() {
    // Duvara çarpma kontrolü
    const wallCollision = walls.some(wall => wall.x === head.x && wall.y === head.y);
    
    // Kendine çarpma kontrolü
    for (let i = 1; i < snake.length; i++) {
        if (head.x === snake[i].x && head.y === snake[i].y) endGame();
    }
}
some() metodu ile duvar çarpışması kontrol edilir

Döngü ile yılanın kendine çarpıp çarpmadığı kontrol edilir

6. Görev Sistemi
   
function setRandomMission() {
    const missions = [
        { type: 'survive', target: 35, text: "35 saniye boyunca hayatta kal" },
        { type: 'collectFruits', target: 0, text: "Bütün meyveleri topla" }
    ];
    // Rastgele görev seçimi
}
Farklı görev tipleri tanımlanmıştır

Her oda için rastgele görev seçilir

7. Güçlendirmeler (Power-ups)

function activateRandomPowerup() {
    const powerups = [
        { name: "Hız Artışı", type: "speedUp", duration: 10000 },
        { name: "Çift Puan", type: "doubleScore", duration: 15000 }
    ];
    // Rastgele güçlendirme seçimi ve etkinleştirme
}
Geçici süreli özel yetenekler içerir

Her biri farklı süre ve etkilere sahiptir

8. Oyun Çizimleri

function drawGame() {
    // Yılan çizimi
    snake.forEach((segment, index) => {
        if (index === 0) {
            // Baş kısmı farklı çiz
        } else {
            // Normal segmentler
        }
    });
    
}
Canvas API kullanılarak tüm oyun elemanları çizilir

Yılanın başı ve gövdesi farklı şekillerde çizilir
