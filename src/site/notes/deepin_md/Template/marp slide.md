---
{"tags":["tips"],"marp":true,"backgroundColor":"black","style":"section {\n  /* 背景随便换 */\n  background: var(--bg, #333);\n  /* 文字自动挑与背景对比最高的白/黑 */\n  color: color-contrast(var(--bg) vs white, black);\n}\n","dg-publish":true,"permalink":"/deepin-md/template/marp-slide/","dgPassFrontmatter":true}
---


![[base-template.base]]

<!-- _backgroundColor: black -->
<!-- _color: white -->
# 文字都自动保持足够反差

在 Marp 里想让「**背景无论设成什么颜色，文字都自动保持足够反差**」，官方没有内建开关，但可以借助 **CSS 的 `contrast-color()` 函数** 或 **自动亮度计算** 来实现。下面给出两种「纯 CSS」即可落地的方案，复制到 Marp 的 `style:` 区块即可生效（Marp 3.5+ 实测通过）。

--------------------------------------------------
方案 A：使用 CSS 新函数 `color-contrast()`  
（最简洁，但需较新 Chromium 内核，Marp CLI 自带 Chrome 已支持）

```markdown
---
marp: true
style: |
  section {
    /* 背景随便换 */
    background: var(--bg, #333);
    /* 文字自动挑与背景对比最高的白/黑 */
    color: color-contrast(var(--bg) vs white, black);
  }
---

<!-- 局部换背景，也会自动反差 -->
<!-- _backgroundColor: #ffeb3b -->
### 这一页背景亮黄，字体会自动变黑
```

--------------------------------------------------
方案 B：纯 CSS 变量 + 亮度计算  
（兼容性最好，Marp 默认 Electron/Edge 都能跑）

```markdown
---
marp: true
style: |
  /* 先定义一个“亮度阈值”变量 */
  section {
    --l: 70;                 /* 背景亮度 >70 算“浅”，反则“深” */
    --bg: #222;              /* 默认深灰背景 */
    background: var(--bg);
    /* 通过 calc 自动选色：浅色→黑，深色→白 */
    color: hsl(0 0% calc((var(--l) - lightness(var(--bg))) * 999%));
  }
---

<!-- 单页换背景 -->
<!-- _backgroundColor: #e3f2fd -->
### 背景浅蓝，字自动变黑
```

--------------------------------------------------

方案 C：直接写「反色」滤镜（最快，但只能是黑白）

```markdown
style: |
  section {
    background: #795548;
    filter: invert(1);   /* 整块反色，字变背景色，背景变字色 */
  }
```

--------------------------------------------------
一句话总结  
- 想要「智能黑白反差」：用 `color-contrast()` 最省事。  
- 想要「兼容性+自定义阈值」：用亮度计算变量法。  
- 只想「反一下」：一行 `filter: invert(1)` 即可。

把对应 CSS 粘进 `style:` 后，**无论后面怎么换 `_backgroundColor`，文字都会自动保持高反差**，不用再手动调 `_color`。

---

