---
layout: post
title: Canvas drawImage
category: Frontend
tags:
  - JavaScript
  - Canvas
  - HTML5
date: 2025-01-09T15:30:00+0900
last_modified_at: 2025-01-09T15:30:00+0900
---

## Canvas drawImage()
> HTML5 Canvas에서 이미지를 그리는 데 사용되는 메소드이다. 이미지를 캔버스에 그리거나, 크기를 조정하거나, 특정 부분만 자르거나, 원하는 위치에 배치할 수 있다.

## 기본 문법

```javascript
context.drawImage(image, dx, dy);
context.drawImage(image, dx, dy, dWidth, dHeight);
context.drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
```

## 파라미터

### image
> 그릴 이미지 객체

- **HTMLImageElement** - `<img>` 요소
- **HTMLCanvasElement** - `<canvas>` 요소  
- **HTMLVideoElement** - `<video>` 요소
- **ImageBitmap** - 비트맵 이미지

### 기본 위치 파라미터
- **dx** - 캔버스에서 이미지가 그려질 x 좌표
- **dy** - 캔버스에서 이미지가 그려질 y 좌표

### 크기 조정 파라미터
- **dWidth** - 캔버스에 그려질 이미지의 너비
- **dHeight** - 캔버스에 그려질 이미지의 높이

### 소스 이미지 자르기 파라미터
- **sx** - 소스 이미지에서 자를 시작 x 좌표
- **sy** - 소스 이미지에서 자를 시작 y 좌표
- **sWidth** - 소스 이미지에서 자를 너비
- **sHeight** - 소스 이미지에서 자를 높이

## 사용 방법

### 1. 기본 이미지 그리기
> 이미지를 원본 크기로 지정된 위치에 그린다.

```javascript
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');
const image = new Image();

image.onload = function() {
    ctx.drawImage(image, 10, 10);
};

image.src = 'example.jpg';
```

### 2. 크기 조정해서 그리기
> 이미지의 크기를 조정하여 그린다.

```javascript
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');
const image = new Image();

image.onload = function() {
    // 이미지를 200x150 크기로 (50, 50) 위치에 그리기
    ctx.drawImage(image, 50, 50, 200, 150);
};

image.src = 'example.jpg';
```

### 3. 이미지 자르기 (Sprite 처리)
> 소스 이미지의 특정 부분만 잘라서 그린다.

```javascript
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');
const spriteImage = new Image();

spriteImage.onload = function() {
    // 소스 이미지의 (100, 100) 위치에서 50x50 크기를 잘라서
    // 캔버스의 (0, 0) 위치에 100x100 크기로 그리기
    ctx.drawImage(spriteImage, 100, 100, 50, 50, 0, 0, 100, 100);
};

spriteImage.src = 'sprite.png';
```

## 활용 예제

### 이미지 비율 유지하며 캔버스에 맞추기

```javascript
function drawImageScaled(ctx, image, x, y, width, height) {
    const imageAspectRatio = image.width / image.height;
    const canvasAspectRatio = width / height;
    
    let renderableHeight, renderableWidth, xStart, yStart;
    
    if (imageAspectRatio < canvasAspectRatio) {
        renderableHeight = height;
        renderableWidth = image.width * (renderableHeight / image.height);
        xStart = (width - renderableWidth) / 2;
        yStart = 0;
    } else if (imageAspectRatio > canvasAspectRatio) {
        renderableWidth = width;
        renderableHeight = image.height * (renderableWidth / image.width);
        xStart = 0;
        yStart = (height - renderableHeight) / 2;
    } else {
        renderableHeight = height;
        renderableWidth = width;
        xStart = 0;
        yStart = 0;
    }
    
    ctx.drawImage(image, x + xStart, y + yStart, renderableWidth, renderableHeight);
}
```

### 이미지 회전하여 그리기

```javascript
function drawRotatedImage(ctx, image, x, y, angle) {
    ctx.save();
    ctx.translate(x + image.width / 2, y + image.height / 2);
    ctx.rotate(angle);
    ctx.drawImage(image, -image.width / 2, -image.height / 2);
    ctx.restore();
}
```

### 스프라이트 애니메이션

```javascript
class SpriteAnimation {
    constructor(image, frameWidth, frameHeight, frameCount) {
        this.image = image;
        this.frameWidth = frameWidth;
        this.frameHeight = frameHeight;
        this.frameCount = frameCount;
        this.currentFrame = 0;
    }
    
    update() {
        this.currentFrame = (this.currentFrame + 1) % this.frameCount;
    }
    
    draw(ctx, x, y) {
        const sourceX = this.currentFrame * this.frameWidth;
        const sourceY = 0;
        
        ctx.drawImage(
            this.image,
            sourceX, sourceY, this.frameWidth, this.frameHeight,
            x, y, this.frameWidth, this.frameHeight
        );
    }
}
```

## 성능 최적화

### 1. 이미지 캐싱
> 같은 이미지를 반복해서 사용할 때는 미리 로드하여 캐싱한다.

```javascript
const imageCache = {};

function loadImage(src) {
    if (imageCache[src]) {
        return Promise.resolve(imageCache[src]);
    }
    
    return new Promise((resolve, reject) => {
        const img = new Image();
        img.onload = () => {
            imageCache[src] = img;
            resolve(img);
        };
        img.onerror = reject;
        img.src = src;
    });
}
```

### 2. 오프스크린 캔버스 활용
> 복잡한 그리기 작업을 오프스크린에서 미리 처리한다.

```javascript
// 오프스크린 캔버스 생성
const offscreenCanvas = document.createElement('canvas');
const offscreenCtx = offscreenCanvas.getContext('2d');

// 복잡한 그리기 작업을 오프스크린에서 처리
function prepareComplexImage() {
    offscreenCanvas.width = 200;
    offscreenCanvas.height = 200;
    
    // 복잡한 그리기 작업들...
    offscreenCtx.fillStyle = 'red';
    offscreenCtx.fillRect(0, 0, 100, 100);
    offscreenCtx.fillStyle = 'blue';
    offscreenCtx.fillRect(100, 100, 100, 100);
}

// 메인 캔버스에 그리기
function drawToMainCanvas(ctx) {
    ctx.drawImage(offscreenCanvas, 0, 0);
}
```

## 주의사항

### 1. 이미지 로드 확인
> 이미지가 완전히 로드된 후에 drawImage를 호출해야 한다.

```javascript
const image = new Image();
image.onload = function() {
    // 이미지 로드 완료 후 그리기
    ctx.drawImage(image, 0, 0);
};
image.src = 'example.jpg';
```

### 2. CORS 정책
> 다른 도메인의 이미지를 사용할 때는 CORS 정책을 고려해야 한다.

```javascript
const image = new Image();
image.crossOrigin = 'anonymous'; // CORS 설정
image.src = 'https://example.com/image.jpg';
```

### 3. 에러 처리
> 이미지 로드 실패에 대한 처리를 해야 한다.

```javascript
const image = new Image();
image.onload = function() {
    ctx.drawImage(image, 0, 0);
};
image.onerror = function() {
    console.error('이미지 로드 실패');
};
image.src = 'example.jpg';
```

## References
- [MDN Web Docs - Canvas API](https://developer.mozilla.org/ko/docs/Web/API/Canvas_API)
- [MDN Web Docs - drawImage()](https://developer.mozilla.org/ko/docs/Web/API/CanvasRenderingContext2D/drawImage)
- [HTML5 Canvas Tutorial](https://www.w3schools.com/html/html5_canvas.asp)