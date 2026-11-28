# 培養皿細菌生長網頁模擬
## 單一菌種生長模擬
[連結：gemini_20261125.html](https://ng00sj.github.io/PetriDish/gemini_20261125)
[連結：gemini_20261128.html](https://ng00sj.github.io/PetriDish/gemini_20261128)
20261128更新：
* 改成可以畫線撒菌（更像抹培養皿）。
* 用 Latex 渲染所用公式（看起來舒服）。
* 增加適應度，以及其依賴的 pH 值（局部）、含氧量（局部）、溫度（全域一致）、濕度（全域一致）。
* 讓細菌有控制 pH 值、含氧量的能力。
* 增加 pH 值、含氧量的分布圖。
* 我調了三個預設模式，分別表現出低度聚集、中度聚集、高度聚集。

## 目前大致內容物
### 環境因子
* pH 值 $\text{pH}_\text{env}$（局部）、含氧量 $O_2$（局部）、溫度 $T$（全域一致）、濕度 $H$（全域一致）、營養 $c$（局部）。

### 其他方程式
* 細菌動態：

$$
\frac{\partial b_i}{\partial t} = \underbrace{D_b^{\text{eff}} \nabla^2 b_i}_{\text{濕度調節擴散}} - \underbrace{\nabla \cdot \left[\chi\, b_i \left(1 - \frac{b_{\text{tot}}}{K}\right)_+ \nabla b_{\text{tot}}\right]}_{\text{守恆體積填充聚集}} + \underbrace{\nabla \cdot \left(M_{\text{ch}} \frac{b_i}{b_{\text{tot}}} \nabla \mu_{\text{ch}}\right)}_{\text{Cahn-Hilliard 相分離}} + \underbrace{\mu(c)\, E(x,y)\, b_i \left(1 - \frac{b_{\text{tot}}}{K}\right)_+ \left(1 + \sigma_\eta\sqrt{\Delta t}\,\eta_i\right)}_{\text{局部環境加權隨機生長}} - \underbrace{d\big(1 - E(x,y)\big)\,b_i}_{\text{逆境衰亡}}
$$

  * 細菌生長：有隨機性、受營養（Monod Model）、適應度影響。
  * 細菌移動：擴散（受濕度影響）、聚集、相分離（Cahn-Hilliard Model）。

$$
\mu_{\text{ch}} = \underbrace{2\phi(1-\phi)(1-2\phi)}_{\text{雙井自由能導數 } f'(\phi)} - \underbrace{\kappa_{\text{ch}}\nabla^2\phi}_{\text{相界表面張力}}
$$

  * 細菌衰亡：依賴於適應度。
* 環境動態：pH 值、含氧量會受細菌改變，營養會補充、擴散、消耗。

$$
\frac{\partial (\text{pH})}{\partial t} = \underbrace{D_{\text{pH}} \nabla^2 (\text{pH})}_{\text{離子擴散}} + \underbrace{\alpha_{\text{pH}} \, \frac{c}{K_c + c} \, b_{\text{tot}}}_{\text{代謝產酸/產鹼}} + \underbrace{\beta_{\text{buf}} (\text{pH}_{\text{env}} - \text{pH})}_{\text{緩衝液平衡}}
$$

$$
\frac{\partial O_2}{\partial t} = \underbrace{D_{O_2} \nabla^2 O_2}_{\text{氧氣擴散}} - \underbrace{q_{O_2} \, \frac{c}{K_c + c} \frac{O_2}{K_{O_2} + O_2} \, b_{\text{tot}}}_{\text{好氧呼吸消耗}} + \underbrace{k_{La} (O_{2,\text{env}} - O_2)}_{\text{空氣溶氧補充}}
$$

$$
\frac{\partial c}{\partial t} = \underbrace{D_c \nabla^2 c}_{\text{營養擴散}} - \underbrace{q\,\mu(c)\,E(x,y)\,b_{\text{tot}}}_{\text{環境加權消耗}} + \underbrace{s(1 - c)}_{\text{基質補充}}, \qquad \mu(c) = \frac{\mu_{\max}\,c}{K_c + c}
$$

* 適應度：依賴於 pH 值、含氧量、溫度、溼度。

$$
E(x,y) = \underbrace{f_T(T)}_{\text{溫度}} \cdot \underbrace{f_{\text{pH}}\big(\text{pH}(x,y)\big)}_{\text{局部酸鹼}} \cdot \underbrace{f_{O_2}\big(O_2(x,y)\big)}_{\text{局部含氧}} \cdot \underbrace{f_H(H)}_{\text{濕度}}
$$

## 說明
* AI 生成的，先拿來作參考。
* 20281128更：目前覺得好像可以拿來用，Gemini 蠻強的。
