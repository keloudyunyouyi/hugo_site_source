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
      typeSpeed = 1000; // 切换到下一句前停留0.5秒
    }
    
    setTimeout(typeEffect, typeSpeed);
  }
  
  // 开始打字效果
  typeEffect();
});
</script>

<style>
.typewriter-container {
  padding: 2rem;
  text-align: center;
}

#typewriter-text {
  font-size: 2rem;
  font-weight: bold;
  border-right: 3px solid;
  white-space: nowrap;
  overflow: hidden;
  display: inline-block; /* 关键：使元素宽度适应内容 */
  vertical-align: middle; /* 确保光标垂直居中 */
  min-height: 1.2em; /* 防止高度闪烁 */
  animation: cursor-blink 0.7s step-end infinite;
}

@keyframes cursor-blink {
  from, to { border-color: transparent }
  50% { border-color: currentColor }
}
</style>
</body></html>

{{< list title="最近"limit=9 cardView=true >}}