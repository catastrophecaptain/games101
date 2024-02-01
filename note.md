# GAMES101笔记

## 前置知识

- 点乘
  - $A\cdot B=|A||B|cos\theta$
  - 矩阵表示：$A\cdot B=A^TB$
  - 应用：求向量角度，相似度，同向性，分解向量

- 叉乘
  - 令$A={x,y,z}$,则矩阵表示为
    $A\times B=\left( \begin{array}{abc} 0 & -z & y \\ z & 0 & -x \\ -y & x & 0\end{array}  \right)B$
  - 应用：判断向量的左右关系，判断点和三角形的内外关系6。

- 齐次坐标
  - 解决平移的特殊情况
  - 点：$(x,y,1)^T=(\omega x,\omega y,\omega )(\omega \neq 0)$
  	向量：$(x,y,0)^T$
  - 仿射变换：
  	$$ \left( \begin{array}{a} x'\\ y' \\1  \end{array} \right) = \left( \begin{array}{aaa} a &b&t_X\\c&d&t_y\\0&0&1 \end{array} \right)\left( \begin{array}a x\\y\\1\end{array} \right)\\ \left( \begin{array}{a} x'\\ y'  \end{array} \right)= \left( \begin{array}{aa} a &b\\c&d \end{array} \right)\left( \begin{array}{a} x\\ y  \end{array} \right) + \left( \begin{array}{a} t_x\\ t_y  \end{array} \right)$$​
  	先变换再平移

- 3维空间
  - 齐次坐标：比二维多一个维度，四维表示
  - 旋转(非齐次坐标)
  	$$\boldsymbol{R}(\boldsymbol{n},\alpha)=\cos\alpha\cdot \boldsymbol{I}+(1-\cos\alpha )\cdot \boldsymbol{n}\boldsymbol{n}^T+\sin\alpha \cdot \left( \begin{array}{abc} 0 & -n_z & n_y \\ n_z & 0 & -n_x \\ -n_y & n_x & 0\end{array}  \right)$$
- 观测变换
  - 相机
    - 参数
      - 位置 $\overrightarrow{e}$
      - 注视方向 $\hat{g}$
      - 向上方向 $\hat{t}$
      - 实际上对应六个自由度
  - 模型视图变换
    - 固定相机位置，让景物移动,保持相对位移不变："The origin, up at Y, look at -Z"
    - 变换矩阵$M_{view}=R_{view}T_{view}$
    - $$T_{view}=\left( \begin{array}{aaaa}  1&0&0&x_{-\overrightarrow{e}} \\0&1&0&y_{-\overrightarrow{e}} \\0&0&1&z_{-\overrightarrow{e}}\\0&0&0&1\end{array}  \right)$$
    - $$R_{view}^{-1}=\left( \begin{array}{aaaa}  x_{\hat{g}\times \hat{t}}&x_{\hat{t}}&x_{-\hat{g}}&0\\y_{\hat{g}\times \hat{t}}&y_{\hat{t}}&y_{-\hat{g}}&0 \\z_{\hat{g}\times \hat{t}}&z_{\hat{t}}&z_{-\hat{g}}&0\\0&0&0&1\end{array}  \right)$$
      	因为$R_{view}$为正交矩阵，$R_{view}=(R_{view}^{-1})^T$
  - 投影变换
    - 正交投影
      ![](./assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-25%20110304.png)
      $$M_{ortho}=\left( \begin{array}{aaaa} \frac{2}{r-l}&0&0&0\\0&\frac{2}{t-b}&0&0\\0&0&\frac{2}{n-f}&0\\0&0&0&1 \end{array}  \right)\left( \begin{array}{aaaa} 1&0&0&-\frac{r+l}{2}\\0&1&0&-\frac{t+b}{2}\\0&0&1&-\frac{n+f}{2}\\0&0&0&1 \end{array}  \right)$$
    - 透视投影 
      - 先做透视到正交的变换，再做正交变换：$$M_{persp}=M_{ortho}M_{persp\to ortho}$$
        ![](./assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-25%20112259.png)
      - 透视到正交变换
        - 约束条件
          1. $y,z$​值符合投影的关系
            ![](./assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-25%20112545.png)
          2. 最近面所有点坐标不变
            ![](./assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-25%20112825.png)
          3. 最远面中心点坐标不变
            ![](./assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-25%20113647.png)
        - 求解结果：
          $$M_{persp \to ortho}=\left( \begin{array}{aaaa} n&0&0&0\\0&n&0&0\\0&0&n+f&-nf\\0&0&1&0 \end{array}  \right)$$
      - 可以采用其他的参数：垂直视角$fovY$​和纵横比，变换矩阵可以等量代换后得出
        ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-25%20164503.png)
  - 视点变换
    $$M_{viewport}=\left( \begin{array}{aaaa} \frac{width}{2}&0&0&\frac{width}{2}\\0&\frac{height}{2}&0&\frac{height}{2}\\0&0&1&0\\0&0&0&1 \end{array}  \right)$$
  - 成像过程
    - 模型变换（放置物体）M
    - 视角变换（放置相机）V
    - 投影变换（转变为标准立方体）P
    - 视点变换（标准立方体到屏幕）

## 光栅化
- 三角形光栅化
  - 所有多边形可以分解成三角形且三角形必然位于一个面上
  - 判断是否在三角形内部函数`inside()`
    - 按照顶点轮换顺序对判别点形成的向量做叉乘，若符号一致则在内部,点在边上情况未定义
    - 举例:
      $${inside}_{\triangle ABC}(P)=\left\{\begin{aligned}&\quad 1\quad 若\overrightarrow{AB}\times \overrightarrow{AP},\overrightarrow{BC}\times \overrightarrow{BP},\overrightarrow{CA}\times \overrightarrow{CP}同正负\\&\quad 0 \quad 其它 \end{aligned}\right.$$
  - 步骤
    1. 取三个顶点的水平垂直最小最大值形成采样区间（Bounding Box）
      ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-25%20175544.png)
    2. 遍历采样区间内的每一个像素中心点（坐标+0.5）对`inside()`函数采样
  - 可以采用增量三角形遍历来应对狭长旋转三角形的情况
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-25%20175745.png)
- 反走样技术(下采样)
  - 走样（Alias）：（信号变化太快但是采样太慢）高频信号采样不足，采样错误地看起来来自低频信号，两个不同频率的信号在给定采样频率下无法分辨
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20091208.png)
  - 傅里叶变换：将信号从频域转换为时域
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20091726.png)
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20094344.png)
    - 时域的卷积相当于频域的乘积，时域的乘积相当于频域的卷积
      ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20094457.png)
    - 均值光滑滤波的卷积核越大，频率就越小
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20095640.png)
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20095711.png)
    - 通过频域理解走样
      - 采样：不断重复频域
        ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20100021.png)
      - 走样：频域发生重叠
        ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20100256.png)
  - 反走样思路
    1. 增大采样频率：昂贵需要更高分辨率的显示器
    2. 减小原图频域长度：滤去高频信息（采样前平均像素点内的颜色）
      ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20100703.png)
      ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20100930.png)
  - MSAA(超采样)：将一个像素区域划分出多个点而非进中心点，判断是否在三角形内，得出所占比例
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20101528.png)
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20101557.png)
  - 其余技术：
    - FXAA：采样后对图像检测边缘再进行处理
    - TAA：采样点有抖动将多次采样分步到时域上去
- 深度检测、
  - 画家算法（缺陷大）：从后往前一个个画三角形，近的覆盖远的 ，且要对三角形排序需$O(n\log{n})$。
  - Z-Buffer算法
    - 通过额外的深度缓冲区存储每个像素点最小深度信息(深度通过三角形顶点插值得到)，只需遍历一遍三角形$O(n)$
      ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20104305.png)
      ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20104345.png)
      ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-29%20104413.png)

## 着色
- 概念：对物体附着材料（对光照的反应）的过程，但不考虑阴影（局部）
  ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-30%20225026.png)
- Blinn-Phong反射模型
  - 思路：将着色分为三个部分：高光区，漫反射区，环境光照区
  
  - 输入：观测角度$\overrightarrow{v}$，表面法向$\overrightarrow{n}$，光照方向$\overrightarrow{l}$，表面参数
  
  - 漫反射相：光照能量随距离平方递减，角度影响接受光照的面积
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-30%20230207.png)
    
  - 高光相：观测方向与反射光方向接近，或者光照方向和观测方向的角平分线方向半程向量$\overrightarrow{h}=\frac{\frac{\overrightarrow{l}+\overrightarrow{v}}{2}}{
    \Vert\frac{\overrightarrow{l}+\overrightarrow{v}}{2}\Vert}$（极易计算）和表面法线方向接近。
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-30%20231524.png)
    - 使用点积来估算方向相关程度，增加p可以使得高光主导区变窄。
      ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-30%20231736.png)
  - 环境光照相：感觉像本底颜色
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-30%20231929.png)
  - 总结
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-30%20232008.png)
- 着色频率
  - 三种着色方式
    - 三角形着色(flat shading):根据三角形法向量和其它参数对三角形内像素着同一种色
    - 顶点着色(Gouraud shading):根据顶点法向量和其它参数对顶点着色,顶点三角形内像素根据顶点颜色插值得出颜色
    - 像素着色(Phong shading):每个像素根据所属三角形顶点的法向量插值得出法向量,再对每个像素进行着色
      ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20110416.png)
  - 顶点法向量:平均周围面的法向量
    $$N_v=\frac{\sum{\omega_i N_i}}{\Vert\sum{\omega_i N_i}\Vert}$$​($\omega_i$​为各个面的权重)
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20111845.png)
- 图形学实时渲染管线
  ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20114519.png)
  ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20114550.png)
  ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20114605.png)
  ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20114628.png)
  ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20114707.png)
  ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20114723.png)
  - shader程序:控制如何对顶点或这fragment着色
- 纹理映射
  - 将三维物体的表面映射到二维平面纹理区域,为着色提供参数
  - 步骤:
    1. 建立纹理坐标系$(u,v)\in [0,1]\times [0,1]$
    2. 将物体表面三角形顶点映射到纹理空间(一般由美工完成,自动化过程仍属于前沿研究),纹理空间可以复用,但需是无缝贴图.
    3. 插值得出三角形内部每个像素的参数
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20123803.png)
- 插值
  - 中心坐标(Barycentric Coordinates)$(\alpha,\beta,\gamma)$
    假设$\triangle_{ABC}$,点$(x,y)$
    $$\left\{\begin{aligned}&\alpha A + \beta B + \gamma C=(x,y)\\ &\alpha + \beta + \gamma =1\end{aligned}\right.$$
    可得重心坐标$(\alpha,\beta,\gamma)$,若符号均为非负,则点在三角形内部.
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20132758.png)
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20132919.png)
  - 根据重心坐标作为权重,可以插值得出其它性质的值
  - 正交投影不改变重心坐标,透视投影改变重心坐标,对于透视投影的插值需要进行透视矫正
- 高级纹理映射
  - 纹理分辨率过小,高分辨屏幕对它进行了放大,产生锯齿(有点像图像处理的scale放大插值,应该任何一张图片将它的一个像素放大到一定程度都会出现锯齿现象)
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20182833.png)
    - 最近邻插值(四舍五入)
    - 双线性插值:选取最近的四个像素点,对目标点所在的一条线端点插值,在对目标点插值
      ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20183051.png)
    - Bicubic插值:选取最近的$4\times 4$像素点,按双线性插值方法查指出四个点,再进行一次双线性插值.
  - 纹理分辨率过大,屏幕分辨率小,或者锥型采样,采样频率过小,造成了走样
    ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20185833.png)
    - 可以使用超采样但是开销过大
    - 三线性插值插值(Mipmap,从点查询到区域查询)
      - Mipmap:存储不同分辨率的图像
        ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20190411.png)
        ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20190429.png)
      - 估算一个像素区域对应纹理区域的大小(可以用下图方法,也可以将四角转换到纹理空间计算边长最大值),得出层数$D$
        ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20190606.png)
      - 三线性插值:根据D的上下取整,分别在两个纹理空间内做双线性插值,再对两个结果进行一次线性插值
        ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20190827.png)
      - 缺点:不同形状的像素对映区域难以用单一正方形表示
    - 各向异性插值:采用长方形而非正方形进行插值
      ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20191016.png)
    - EWA filtering:采用椭圆,多次采样,未详细讲
      ![](assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-01-31%20191150.png)