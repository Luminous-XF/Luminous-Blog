# 两圆相交

## 两圆相交求面积

有 A、B 两个圆，圆 A 半径为 R，圆 B 的半径为 r。求圆 A 与 圆 B 相交部分 $area$ （下图红色区域）的面积。

![图1](ACM-Algorithm-几何-2D-两圆相交-001.png)

![图2](ACM-Algorithm-几何-2D-两圆相交-002.png)

$S_{area} ~ = ~ 扇形_{CAD} ~ + ~ 扇形_{CBD} ~ - ~四边形_{CADB}$

先通过余弦定理求出 $\theta_{1}~(\angle{CAD})$ 与 $\theta_{2}~(\angle{CBD})$ 的值

$\theta_{1} ~ = ~ 2 ~ \cdot ~ \arccos(\frac{AC^{2} ~ + ~AB^{2} ~ - ~ CB^{2}}{2 ~ \cdot ~ AC ~ \cdot ~ AB})$

$\theta_{2} ~ = ~ 2 ~ \cdot ~ \arccos(\frac{CB^{2} ~ + ~AB^{2} ~ - ~ AC^{2}}{2 ~ \cdot ~ CB ~ \cdot ~ AB})$

然后求 $扇形_{CAD}$、$扇形_{CBD}$、 $四边形_{CADB}$ 的面积

$扇形_{CAD} ~ = ~ \frac{1}{2} ~ \cdot ~ \theta_{1} ~ R^{2}$

$扇形_{CBD} ~ = ~ \frac{1}{2} ~ \cdot ~ \theta_{2} ~ r^{2}$

$四边形_{CADB} ~ = ~ \frac{1}{2} ~ \cdot ~ CA ~ \cdot ~ AB ~ \cdot ~ \sin(\frac{\theta_{1}}{2})$

<br/>

## 算法实现

$时间复杂度: O(1)$
<br/>
$空间复杂度: O(1)$

```C++
const double PI = acos(-1);

struct Circle {
    double x, y; // 圆心
    double r;    // 半径
};

double getDis(double x1, double y1, double x2, double y2) {
    return sqrt((x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1));
}

/**
 * 计算 a、b 两圆相交部分面积
 */
double getArea(Circle a, Circle b) {
    // 假设圆 a 为半径较大的圆, 圆 b 为半径较小的圆
    if (a.r < b.r) swap(a, b);

    // 圆心距
    double dis = getDis(a.x, a.y, b.x, b.y);

    // 两圆相离或相切则无重叠部分, 返回 0
    if (dis >= a.r + b.r) return 0;

    // 两圆相含(圆 b 在 圆 a 内), 重叠部分为 圆 b 的面积
    if (dis <= a.r - b.r) return PI * b.r * b.r;

    double theta1 = 2 * acos((a.r * a.r + dis * dis - b.r * b.r) / (2 * a.r * dis)); // 圆 a 圆心角
    double theta2 = 2 * acos((b.r * b.r + dis * dis - a.r * a.r) / (2 * b.r * dis)); // 圆 b 圆心角
    double area1 = theta1 * a.r * a.r / 2; // 圆 a 扇形面积
    double area2 = theta2 * b.r * b.r / 2; // 圆 b 扇形面积
    double area3 = a.r * dis * sin(theta1 / 2); // 四边形面积
    double area = area1 + area2 - area3;

    return area;
}
```

## 相关例题

| 问题                                                                         | 类型         | 难度      | 题解                                   |
|----------------------------------------------------------------------------|------------|---------|--------------------------------------|
| [UVA 358 - Don't Have A Cow](https://onlinejudge.org/external/3/358.pdf)   | 几何 2D、二分查找 | Level 4 | [Solution](ACM-Solution-UVA-358.md)  |
| [HDU 5120 - Intersection](https://acm.hdu.edu.cn/showproblem.php?pid=5120) | 几何 2D      | Level 3 | [Solution](ACM-Solution-HDU-5120.md) |
