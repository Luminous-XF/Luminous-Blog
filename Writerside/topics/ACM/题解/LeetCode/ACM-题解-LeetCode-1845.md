# LeetCode 1845. 座位预约管理系统

## 原题地址

[](https://leetcode.cn/problems/seat-reservation-manager)

<br/>

## 题目类型

模拟

<br/>

## 代码

时间复杂度: $O(\log{n})$
<br/>
空间复杂度: $O(n)$

```C++
class SeatManager {
public:
    map<int, bool> seat;

    SeatManager(int n) {
        for (int i = 1; i <= n; i++) {
            seat[i] = true;
        }
    }

    int reserve() {
        int res = seat.begin()->first;
        seat.erase(seat.begin());
        return res;
    }

    void unreserve(int seatNumber) {
        seat[seatNumber] = true;
    }
};

/**
 * Your SeatManager object will be instantiated and called as such:
 * SeatManager* obj = new SeatManager(n);
 * int param_1 = obj->reserve();
 * obj->unreserve(seatNumber);
 */
```