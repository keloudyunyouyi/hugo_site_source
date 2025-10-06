---
description: "一个...博客网站"
---
一个....额....一个个人博客网站

目前还什么都没有但
<div class="typewriter-container">
  <h2 id="typewriter-text"></h2>
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const textElement = document.getElementById('typewriter-text');
  const texts = [
    "Change is the only constant",
    "唯一不变的是改变"
  ];
  
  let textIndex = 0;
  let charIndex = 0;
  let isDeleting = false;
  
  function typeEffect() {
    const current = textIndex % texts.length;
    const currentText = texts[current];
    
    if (isDeleting) {
      // 删除文字
      textElement.textContent = currentText.substring(0, charIndex - 1);
      charIndex--;
    } else {
      // 输入文字
      textElement.textContent = currentText.substring(0, charIndex + 1);
      charIndex++;
    }
    
    // 速度设置
    let typeSpeed = 80;
    
    if (isDeleting) {
      typeSpeed /= 2; // 删除速度更快
    }
    
    // 如果输入完成
    if (!isDeleting && charIndex === currentText.length) {
      isDeleting = true;
      typeSpeed = 1000; // 停留1秒后开始删除
    } else if (isDeleting && charIndex === 0) {
      isDeleting = false;
      textIndex++;
      typeSpeed = 1000; // 切换到下一句前停留1秒
    }
    
    setTimeout(typeEffect, typeSpeed);
  }
  
  // 开始打字效果
  typeEffect();
});
</script>

<style>
.typewriter-container {
  padding: 1rem 1rem; /* 移动端减少左右内边距 */
  text-align: center;
  max-width: 100%; /* 适应Hugo容器宽度 */
  box-sizing: border-box; /* 确保内边距不影响整体宽度 */
}

#typewriter-text {
  /* 响应式字号 - 在不同屏幕尺寸下自动调整 */
  font-size: clamp(1rem, 5vw, 2rem);
  font-weight: bold;
  border-right: clamp(2px, 0.5vw, 3px) solid; /* 响应式光标宽度 */
  white-space: nowrap;
  overflow: hidden;
  display: inline-block;
  vertical-align: middle;
  min-height: 1.2em;
  animation: cursor-blink 0.7s step-end infinite;
}

@keyframes cursor-blink {
  from, to { border-color: transparent }
  50% { border-color: currentColor }
}

/* 小屏设备适配 */
@media (max-width: 480px) {
  .typewriter-container {
    padding: 1.5rem 0.5rem;
  }
  
  /* 极端情况下允许文字换行 */
  @media (max-width: 320px) {
    #typewriter-text {
      white-space: normal;
      border-right: none;
    }
  }
}
</style>

{{< list title="最近"limit=9 cardView=true >}}