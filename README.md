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
  }
}
