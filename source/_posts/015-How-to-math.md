---
title: How to make Hexo support mathematical formulas
date: 2020-04-30 
category: Front-end
tags:
- hexo
- Latex

---

Mathematical formulas are often used when we blog. But we will find bugs in Hexo's support for mathematical formulas.

By default, Hexo uses the `hexo-render-marked` engine to render web pages. This engine will convert some special `markdown` symbols into corresponding `html` tags.

When we write mathematical formulas in Latex-like format, some meaningful symbols will conflict with the engine. We can solve this problems by replacing the engine.

Download `pandoc`:
```shell
sudo pacman -S Pandoc
```

Install `hexo-renderer-pandoc`:
```shell
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-pandoc --save
```

Redeploy blog

$$\left(x^{2}+y^{2}-1\right)^{3}=x^{2}y^{3}$$

$$x_1, x_2, ..., x_n$$
