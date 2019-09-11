# particle_filter的改进
## 使landmarks周期移动
对应文件：periodic_landmarks.py

思路：每次循环使landmarks向右移动1px，以窗口宽度800为周期。

问题：从代码上来看，鼠标移动前后的weights是不会相互影响的，但是当鼠标停止，landmars移动时会有影响吗？从运行结果上来看，对于水平移动的landmarks，即y坐标不变的情况下，PF定位的结果也是y坐标更趋向于实际值，x坐标会表现出更大的误差，图形上的表现就是particles分布在实际坐标附近的横向带状区域内，如下图所示：

![1.png](https://i.loli.net/2019/09/11/ugwIG78MzkL6hNf.png)

## 使用泊松分布计算weights
对应文件：weights_Poisson.py

思路:当二项分布的重复次数n足够大时可以用泊松分布近似二项分布，而当泊松分布的$\lambda$足够大时，可用高斯分布近似泊松分布。而在这个程序中泊松分布的期望普遍在100以上甚至更高，因此符合近似条件。即在update函数中对使用的分布进行修改即可，但是要注意泊松分布是离散型的，在非整数点上的取值为0，因此对于pmf函数的自变量要取整。
    
## 将particles的位置取平均，作为最终定位的位置输出到屏幕上
对应文件：display_coor.py

思路：作者已经在estimate函数中实现了该部分的功能，但是有一个小错误，只计算了particles的x坐标，改正即可

## 将三个改进综合到一起
对应文件：PF_poisson.py