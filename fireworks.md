---
title: 點擊煙花特效
published: 2026-10-02
description: 搬運自Night1918「原文章blog無法訪問，該程序碼是在個人整理收藏時發現」
tags: [Fireworks, ClickMotion]
category: JavaScript
draft: false
pinned: false
comment: true  
---

# 程式碼部分

```javascript
(() => {
  if (window.fireworksInitialized) {
			// 检查是否已经初始化过
			return;
		}

		// 标记为已初始化
		window.fireworksInitialized = true;

		// 清理函数
		function cleanupFireworks() {
			if (window.fireworksCanvas) {
				document.body.removeChild(window.fireworksCanvas);
				window.fireworksCanvas = null;
			}
			if (window.fireworksAnimation) {
				window.fireworksAnimation.pause();
				window.fireworksAnimation = null;
			}
			if (window.fireworksClickHandler) {
				document.removeEventListener("click", window.fireworksClickHandler);
				window.fireworksClickHandler = null;
			}
			if (window.fireworksResizeHandler) {
				window.removeEventListener("resize", window.fireworksResizeHandler);
				window.fireworksResizeHandler = null;
			}
		}

		// 在页面卸载时清理
		window.addEventListener("beforeunload", cleanupFireworks);

		// 创建canvas元素
		var e = document.createElement("canvas");
    e.id = "fireworks-canvas";
		(e.style.cssText =
			"position:fixed;top:0;left:0;pointer-events:none;z-index:9999999"),
			document.body.appendChild(e);
      // 保存canvas引用到全局变量
				window.fireworksCanvas = e;
		var n = e.getContext("2d");
		var a = 0;
		var t = 0;
		var i = [
  "rgba(230, 245, 255, 0.9)",  // #E6F5FF
  "rgba(176, 214, 255, 0.9)",  // #B0D6FF
  "rgba(224, 243, 255, 0.9)",  // #E0F3FF
  "rgba(135, 189, 255, 0.9)",  // #87BDFF
];
		function r() {
			(e.width = 2 * window.innerWidth),
				(e.height = 2 * window.innerHeight),
				(e.style.width = `${window.innerWidth}px`),
				(e.style.height = `${window.innerHeight}px`),
				e.getContext("2d").scale(2, 2);
		}
		function d(e, a) {
			var t = {};
			return (
				(t.x = e),
				(t.y = a),
				(t.color = i[anime.random(0, i.length - 1)]),
				(t.radius = anime.random(16, 32)),
				(t.endPos = ((e) => {
					var n = (anime.random(0, 360) * Math.PI) / 180;
					var a = anime.random(50, 180);
					var t = [-1, 1][anime.random(0, 1)] * a;
					return { x: e.x + t * Math.cos(n), y: e.y + t * Math.sin(n) };
				})(t)),
				(t.draw = () => {
					n.beginPath(),
						n.arc(t.x, t.y, t.radius, 0, 2 * Math.PI, !0),
						(n.fillStyle = t.color),
						n.fill();
				}),
				t
			);
		}
		function o(e) {
			for (var n = 0; n < e.animatables.length; n++)
				e.animatables[n].target.draw();
		}
		function l(e, a) {
			for (
				var t = ((e, a) => {
						var t = {};
						return (
							(t.x = e),
							(t.y = a),
							(t.color = "#FFF"),
							(t.radius = 0.1),
							(t.alpha = 0.5),
							(t.lineWidth = 6),
							(t.draw = () => {
								(n.globalAlpha = t.alpha),
									n.beginPath(),
									n.arc(t.x, t.y, t.radius, 0, 2 * Math.PI, !0),
									(n.lineWidth = t.lineWidth),
									(n.strokeStyle = t.color),
									n.stroke(),
									(n.globalAlpha = 1);
							}),
							t
						);
					})(e, a),
					i = [],
					r = 0;
				r < 30;
				r++
			)
				i.push(d(e, a));
			anime
				.timeline()
				.add({
					targets: i,
					x: (e) => e.endPos.x,
					y: (e) => e.endPos.y,
					radius: 0.1,
					duration: anime.random(1200, 1800),
					easing: "easeOutExpo",
					update: o,
				})
				.add(
					{
						targets: t,
						radius: anime.random(80, 160),
						lineWidth: 0,
						alpha: {
							value: 0,
							easing: "linear",
							duration: anime.random(600, 800),
						},
						duration: anime.random(1200, 1800),
						easing: "easeOutExpo",
						update: o,
					},
					0,
				);
		}
		var s = anime({
			duration: Number.POSITIVE_INFINITY,
			update: () => {
				n.clearRect(0, 0, e.width, e.height);
			},
		});
		document.addEventListener(
  "pointerdown",
  (e) => {
    if (e.button !== 0 && e.pointerType === "mouse") return;
    s.play();
    a = e.clientX;
    t = e.clientY;
    l(a, t);
  },
  { passive: true }
),
  r(),
  window.addEventListener("resize", r, !1);
})();
```

# 小特性

由於iOS的click事件要等500ms才能響應，這裡改成了pointerdown。所以滑動螢幕手指點的地方也會有煙花^_^

## 原作者B站：https://b23.tv/AVswRkD