---
title: "相机标定及棋盘格标定法详解"
date: 2025-05-20T18:34:25+08:00
description: 本文介绍相机标定的概念及棋盘格标定法。
draft: false
math: true
categories:
- 计算机
- 编程
tags:
- CV
- 相机标定
---

# 相机标定及棋盘格标定法详解

## 什么是相机标定？

相机标定（Camera Calibration）是指通过一组已知空间几何结构的物体（如棋盘格）拍摄图像，计算出相机的内参矩阵和畸变系数的过程。

这一步使得我们能够从二维图像像素坐标恢复三维空间信息，或者进行精准的图像几何校正。标定结果通常包括：

- **相机内参矩阵** $K$  
  $$K = \begin{bmatrix} f_x & 0 & c_x \ 0 & f_y & c_y \ 0 & 0 & 1 \ \end{bmatrix}$$ 其中，$f_x, f_y$ 是水平和竖直方向上的焦距（像素单位），$c_x, c_y$ 是主点坐标。

- **畸变系数** $D$  
  包含径向和切向畸变参数，如 $k_1, k_2, p_1, p_2, k_3$ 等。

- **旋转和平移向量** 用于描述相机坐标系与世界坐标系之间的位置关系。

相机标定是计算相机内外参的过程。通过标定，我们能获得：

- **内参（Intrinsic Parameters）**：相机的固有属性，如焦距$(f_x, f_y)$、光心位置$(c_x, c_y)$和畸变参数。
- **外参（Extrinsic Parameters）**：相机相对于世界坐标系的位置和朝向，即旋转矩阵$R$和平移向量$t$。

标定的目标是建立3D物体坐标与图像坐标之间的映射关系。

在计算机视觉中，标定后的相机矩阵$K$通常表示为：

$$K = \begin{bmatrix} f_x & 0 & c_x \ 0 & f_y & c_y \ 0 & 0 & 1 \ \end{bmatrix}$$

结合畸变参数，可以纠正成像中的径向和切向畸变。

---

## 棋盘格标定法

棋盘格标定法是工业界和学术界应用最为广泛的标定方式，因其角点容易定位且结构规则。

### 简介

棋盘格标定法利用具有已知几何形状的二维平面棋盘作为标定样板。其关键步骤包括：

1. **准备棋盘格**  
   典型棋盘由黑白相间的方格组成，角点（内角交点）作为标定点。

2. **在多张不同角度的棋盘图像中检测角点**  
   使用OpenCV的`findChessboardCorners()`函数自动定位棋盘格内角点。

3. **亚像素级角点精确化**  
   通过`cornerSubPix()`精细调整角点位置，提高标定精度。

4. **对应三维空间点与二维图像点**  
   三维点多为棋盘格上预先定义的规则坐标，例如$(0,0,0),(1,0,0),\ldots$。

5. **调用`calibrateCamera()`计算相机内外参**  
   通过最小化重投影误差的优化方法，求得相机参数。

### 核心步骤：

- 准备一个具有多个方格单元的棋盘格标定板，已知单元格大小（如 $20 \times 20$ 毫米）。
- 采集多张棋盘格在不同姿态的图像。
- 使用角点检测算法找到每张图像中棋盘格的角点（图像点）。
- 结合棋盘格的物理坐标（空间点），利用对应点进行相机参数求解。

### 数学模型

在针孔相机模型下，三维空间点 $P_w = (X_w, Y_w, Z_w)$ 通过变换到相机坐标系后投影到像素坐标 $p = (u, v)$：

$$s \begin{bmatrix} u \ v \ 1 \ \end{bmatrix} = K \begin{bmatrix} R & t \end{bmatrix} \begin{bmatrix} X_w \ Y_w \ Z_w \ 1 \ \end{bmatrix}$$

其中：

- $s$ 是尺度因子。
- $R$ 是旋转矩阵，$t$ 是平移向量。
- $K$ 是内参矩阵。

根据上述方程，给定一组标定点的空间坐标及其对应像素点，利用非线性优化（如Levenberg-Marquardt）求解 $K$、$R$、$t$ 和畸变参数。

### 投影模型

世界坐标系中的3D点$X_w = [X,Y,Z,1]^T$通过相机坐标变换和投影变为图像点$x = [u,v]^T$：

$$s \begin{bmatrix} u \ v \ 1 \end{bmatrix} = K [R \quad t] \begin{bmatrix} X \ Y \ Z \ 1 \end{bmatrix}$$

其中：

- $K$为相机内参矩阵
- $R$为3×3旋转矩阵
- $t$为3×1平移向量
- $s$为尺度因子

### 重投影误差

最小化目标是重投影误差：

$$\min_{K, R_i, t_i} \sum_{i=1}^N \sum_{j=1}^M | x_{ij} - \hat{x}_{ij} |^2$$

其中：

- $x_{ij}$为第$i$幅图中第$j$个检测到的角点
- $\hat{x}_{ij}$为第$i$幅图对应的3D点经过投影后的点

优化完成后，即得到相机的内外参数和畸变系数。


## 相机标定Python实现及说明

下面代码通过模块化和详细注释，提高代码的可读性和复用性。

```python
import numpy as np
import cv2 as cv
import glob

def prepare_object_points(pattern_size):
    """
    生成棋盘格的三维世界坐标点，以平面（z=0）上的规则点。
    
    Parameters:
        pattern_size (tuple): 风格为（内角点列数, 内角点行数） 
        
    Returns:
        objp (ndarray): 形如(N, 3)，每行为一个3D点
    """
    objp = np.zeros((pattern_size[1] * pattern_size[0], 3), np.float32)
    objp[:, :2] = np.mgrid[0:pattern_size[0], 0:pattern_size[1]].T.reshape(-1, 2)
    return objp

def find_corners(images, pattern_size, criteria):
    """
    在多张图像中寻找棋盘格角点，并做亚像素级精细调整
    
    Parameters:
        images (list): 图像路径列表
        pattern_size (tuple): 棋盘格内角点尺寸 (cols, rows)
        criteria (tuple): 角点精细化迭代终止标准
        
    Returns:
        objpoints (list): 3D点世界坐标列表
        imgpoints (list): 2D图像坐标点列表
        img_shape (tuple): 图像尺寸(宽, 高)
    """
    objp = prepare_object_points(pattern_size)
    objpoints = []  # 三维点列表
    imgpoints = []  # 二维点列表

    for fname in images:
        img = cv.imread(fname)
        if img is None:
            print(f"Warning: unable to load image {fname}")
            continue
        gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
        ret, corners = cv.findChessboardCorners(gray, pattern_size, None)
        
        if ret:
            # 亚像素级角点精细化
            corners_refined = cv.cornerSubPix(gray, corners, (11, 11), (-1, -1), criteria)
            objpoints.append(objp)
            imgpoints.append(corners_refined)

            # 可视化检测效果
            cv.drawChessboardCorners(img, pattern_size, corners_refined, ret)
            cv.imshow('Corners Detection', img)
            cv.waitKey(500)  # 展示500ms
    cv.destroyAllWindows()

    if not imgpoints:
        raise RuntimeError("未检测到任何有效棋盘格角点！请检查图片路径及参数设置。")
    return objpoints, imgpoints, gray.shape[::-1]

def calibrate_camera(objpoints, imgpoints, image_size):
    """
    利用世界坐标和图像坐标计算相机内参和畸变参数
    
    Parameters:
        objpoints (list): 三维点集列表
        imgpoints (list): 二维点集列表
        image_size (tuple): 图像尺寸 (宽, 高)
        
    Returns:
        ret (float): RMS重投影误差
        mtx (ndarray): 相机矩阵
        dist (ndarray): 畸变系数
        rvecs (list): 旋转向量列表
        tvecs (list): 平移向量列表
    """
    ret, mtx, dist, rvecs, tvecs = cv.calibrateCamera(
        objpoints, imgpoints, image_size, None, None
    )
    return ret, mtx, dist, rvecs, tvecs

def compute_reprojection_error(objpoints, imgpoints, rvecs, tvecs, mtx, dist):
    """
    计算所有图像的平均重投影误差，衡量标定精度
    
    Returns:
        mean_error (float): 平均误差
    """
    total_error = 0
    for i in range(len(objpoints)):
        imgpoints_proj, _ = cv.projectPoints(objpoints[i], rvecs[i], tvecs[i], mtx, dist)
        error = cv.norm(imgpoints[i], imgpoints_proj, cv.NORM_L2) / len(imgpoints_proj)
        total_error += error
    mean_error = total_error / len(objpoints)
    return mean_error

def main():
    # 棋盘内角点尺寸(列, 行)
    pattern_size = (24, 13)
    
    # 迭代终止条件
    criteria = (cv.TERM_CRITERIA_EPS + cv.TERM_CRITERIA_MAX_ITER, 30, 0.001)

    # 读取所有标定图像路径，确保图像覆盖不同角度和视场
    images = glob.glob('data/cali/*.jpg')
    
    objpoints, imgpoints, image_size = find_corners(images, pattern_size, criteria)
    ret, mtx, dist, rvecs, tvecs = calibrate_camera(objpoints, imgpoints, image_size)
    
    mean_error = compute_reprojection_error(objpoints, imgpoints, rvecs, tvecs, mtx, dist)

    print(f"标定完成，重投影误差 (RMS): {ret}")
    print(f"平均重投影误差: {mean_error}")
    print(f"相机矩阵:\n{mtx}")
    print(f"畸变系数:\n{dist.ravel()}")

    # 保存参数，方便未来加载与使用
    np.savez('camera_calibration.npz', mtx=mtx, dist=dist, rvecs=rvecs, tvecs=tvecs)

if __name__ == "__main__":
    main()
```

---

小结：

- **相机标定**是获取相机准确内外参数的基础，广泛应用于机器视觉、三维重建等领域。
- **棋盘格标定法**因其简单直观、角点检测稳定，是最常用的标定方法。
- 通过多视角棋盘图像，检测角点，亚像素精细化，配合OpenCV的`calibrateCamera`函数完成相机参数估计。
- 最终通过重投影误差评估标定结果的准确性，误差越小越好。
- 优化代码结构可以提升易懂性与复用性，便于实际工业或科研项目中的应用。


## 姿态估计的Python代码优化与详解


```python
import numpy as np
import cv2 as cv
import glob

def load_calibration_data(filepath='B.npz'):
    """加载相机内参和畸变参数"""
    with np.load(filepath) as data:
        mtx = data['mtx']
        dist = data['dist']
    return mtx, dist

def draw_cube(img, imgpts):
    """
    在图像上绘制3D立方体的投影
    - imgpts: 8个立方体顶点的二维投影点
    """
    imgpts = np.int32(imgpts).reshape(-1, 2)

    # 绘制底面（绿色填充）
    img = cv.drawContours(img, [imgpts[:4]], -1, (0, 255, 0), -3)
    # 绘制柱子（蓝色线条）
    for i, j in zip(range(4), range(4, 8)):
        img = cv.line(img, tuple(imgpts[i]), tuple(imgpts[j]), (255, 0, 0), 3)
    # 绘制顶部（红色线条）
    img = cv.drawContours(img, [imgpts[4:]], -1, (0, 0, 255), 3)
    return img

def main():
    # 加载相机参数
    mtx, dist = load_calibration_data('B.npz')

    # 设置棋盘格大小，这里(24, 13)表示24×13个内角点
    chessboard_size = (24, 13)

    # 生成世界坐标系中的棋盘格点，Z=0平面
    objp = np.zeros((chessboard_size[0] * chessboard_size[1], 3), np.float32)
    objp[:, :2] = np.mgrid[0:chessboard_size[0], 0:chessboard_size[1]].T.reshape(-1, 2)

    # 设置亚像素角点检测的终止条件
    criteria = (cv.TERM_CRITERIA_EPS + cv.TERM_CRITERIA_MAX_ITER, 30, 0.001)

    # 定义一个3D立方体的8个顶点，用于投影
    cube_size = 3  # 立方体边长，单位与棋盘格点一致
    axis = np.float32([
        [0, 0, 0], [0, cube_size, 0], [cube_size, cube_size, 0], [cube_size, 0, 0],
        [0, 0, -cube_size], [0, cube_size, -cube_size], [cube_size, cube_size, -cube_size], [cube_size, 0, -cube_size]
    ])

    # 读取所有测试图像
    image_files = glob.glob('data/pose/*.jpg')

    for fname in image_files:
        img = cv.imread(fname)
        gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)

        # 寻找棋盘格角点
        ret, corners = cv.findChessboardCorners(gray, chessboard_size, None)
        if not ret:
            print(f"未检测到角点: {fname}")
            continue

        # 亚像素级角点优化
        corners_subpix = cv.cornerSubPix(
            gray, corners, winSize=(11, 11), zeroZone=(-1, -1), criteria=criteria
        )

        # 使用solvePnP估计姿态
        ret, rvec, tvec = cv.solvePnP(objp, corners_subpix, mtx, dist)
        if not ret:
            print(f"姿态估计失败: {fname}")
            continue

        # 将3D立方体顶点投影到图像平面
        imgpts, _ = cv.projectPoints(axis, rvec, tvec, mtx, dist)

        # 绘制立方体
        img = draw_cube(img, imgpts)

        # 显示结果图像
        cv.imshow('Pose Estimation', img)
        key = cv.waitKey(0) & 0xFF
        if key == ord('s'):  # 按's'键保存结果图
            save_path = fname.rsplit('.', 1)[0] + '_pose.png'
            cv.imwrite(save_path, img)
            print(f'图像保存为 {save_path}')

    cv.destroyAllWindows()

if __name__ == "__main__":
    main()
```

---

代码详解和优化说明：

1. 结构化设计

- 将标定数据加载封装成函数 `load_calibration_data` 便于复用和维护。
- 使用 `main()` 函数组织流程，清晰分离功能模块。
- 处理每张图像时，增加角点检测成功与否的判断，输出友好提示。

2. 变量和参数说明

- **chessboard_size** 定义棋盘格内角点数目，和标定板物理结构一致。
- **objp** 是棋盘格角点的三维世界坐标（假定Z=0平面），匹配图像角点对应关系。
- `criteria` 为亚像素角点优化的迭代终止条件，保证检测角点高精度。
- **cube_size** 方便调整立方体尺寸。

3. 绘制加强

- 绘制时区分底面、边柱和顶面，颜色更直观（绿色底面、蓝色柱子、红色顶面）。
- 线宽和填充颜色根据功能区分，提升视觉效果。

4. 交互体验

- 支持手动查看每张图像
- 支持按`s`键保存图像，方便批量结果保存。

---

# 总结

本文介绍了相机标定的基本原理和基于棋盘格的经典标定方法，结合数学模型重点阐述了相机内参数与变换关系。

通过重写的姿态估计代码，提供了实际如何利用OpenCV接口进行姿态求解与3D物体投影的实践示例，代码更规范，注释及变量命名清晰，便于学习和扩展。














