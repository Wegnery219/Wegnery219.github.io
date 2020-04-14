---
title: linear algebra homework
tags: homework
comment: true
date: 2019-10-08 12:09:00
description: 国科大线性代数课程大作业，线性代数plu，qr,household,givens分解算法的python代码。
---
### plu分解

python:
exchange函数如果不用np.copy会导致改完之后的两行是一样的，因为python传的是引用。
```
import numpy as np

'''
交换矩阵中的两行
'''
def exchange(mat, rowi, rowj):
    tmp = np.copy(mat[rowi,:])
    mat[rowi,:] = mat[rowj,:]
    mat[rowj,:] = tmp

'''
PLU分解函数
'''
def LUDecompose (matrix): 
    rows,columns=np.shape(matrix)
    L=np.zeros((rows,columns))
    U=np.zeros((rows,columns))
    P=np.identity(columns)

    for i in range(rows):
        maxindex = np.argmax(np.abs(matrix[i:,i]))
        maxindex = maxindex+i
        if maxindex!=i:
            exchange(matrix, maxindex, i)
            exchange(L,maxindex,i)
            exchange(P,maxindex,i)
        L[i][i]=1
        for row in range(i+1, rows):
            param = matrix[row][i]/matrix[i][i]
            L[row][i]=param
            for j in range(i+1, columns):
                matrix[row][j]=(-param)*matrix[i][j]+matrix[row][j]
    for i in range(rows):
        for j in range(i,columns):
            U[i][j]=matrix[i][j]
    return P,L,U

'''
PLU 分解：
1. 将matrix修改成需要测试的矩阵，如main函数中的matrix所示，注意dtype=float
2. 运行.py文件，python LU.py或者直接在ide中运行，输出P,L,U
注：
输入矩阵必须是平方阵
需要安装依赖库numpy
'''
if __name__=="__main__":
    matrix = np.array([[1,2,-3,4],
                       [4,8,12,-8],
                       [2,3,2,1],
                       [-3,-1,1,-4]], dtype=float)
    print("original matrix:")
    print(matrix)
    # 判断是否是平方阵
    rows,columns = np.shape(matrix)
    if rows!=columns:
        print("matrix need to be a square matrix")
        exit(-1)
    else:
        P,L,U=LUDecompose(matrix)
        print("P:")
        print(P)
        print("L:")
        print(L)
        print("U:")
        print(U)
```

### 全部分解
根据程序提示，输入1为plu分解，输入2为QR分解，输入3为household，输入4为givens，输入5为退出程序。

如果想修改矩阵查看某个分解对于不同矩阵的效果，请在Decompose.py中的main函数中找到对应分解(PLU,QR,Household,Givens)，修改其中的matrix

如果不修改原始矩阵显示输出为如下所示：
Welcome to Decompose program
Input the Number of the Decompose
1.PLU  2.QR  3.Household  4.Givens 5.quit program
By the way,if you wanna to change original matrix, you need to alter it in the python file
Input Number:1
original PLU matrix:
[[ 1.  2. -3.  4.]
 [ 4.  8. 12. -8.]
 [ 2.  3.  2.  1.]
 [-3. -1.  1. -4.]]
P:
[[0. 1. 0. 0.]
 [0. 0. 0. 1.]
 [1. 0. 0. 0.]
 [0. 0. 1. 0.]]
L:
[[ 1.          0.          0.          0.        ]
 [-0.75        1.          0.          0.        ]
 [ 0.25        0.          1.          0.        ]
 [ 0.5        -0.2         0.33333333  1.        ]]
U:
[[  4.   8.  12.  -8.]
 [  0.   5.  10. -10.]
 [  0.   0.  -6.   6.]
 [  0.   0.   0.   1.]]

Input Number:2
origin QR matrix:
[[  0. -20. -14.]
 [  3.  27.  -4.]
 [  4.  11.  -2.]]
Q:
[[ 0.   -0.8  -0.6 ]
 [ 0.6   0.48 -0.64]
 [ 0.8  -0.36  0.48]]
R:
[[ 5. 25. -4.]
 [ 0. 25. 10.]
 [ 0.  0. 10.]]

Input Number:3
origin Household matrix:
[[  0. -20. -14.]
 [  3.  27.  -4.]
 [  4.  11.  -2.]]
R:
[[ 5. 25. -4.]
 [ 0. 25. 10.]
 [ 0.  0. 10.]]
Q:
[[ 0.    0.6   0.8 ]
 [-0.8   0.48 -0.36]
 [-0.6  -0.64  0.48]]

Input Number:4
origin Givens matrix:
[[  0. -20. -14.]
 [  3.  27.  -4.]
 [  4.  11.  -2.]]
R:
[[ 5. 25. -4.]
 [ 0. 25. 10.]
 [ 0. -0. 10.]]
Q:
[[ 0.    0.6   0.8 ]
 [-0.8   0.48 -0.36]
 [-0.6  -0.64  0.48]]
 
Input Number:5
GoodBye

注：其中的例子均为老师上课ppt中的原样例

#### python code
```
import numpy as np 

'''
交换矩阵中的两行
'''
def exchange(mat, rowi, rowj):
    tmp = np.copy(mat[rowi,:])
    mat[rowi,:] = mat[rowj,:]
    mat[rowj,:] = tmp

'''
PLU分解函数
'''
def LUDecompose (matrix): 
    rows,columns=np.shape(matrix)
    L=np.zeros((rows,columns))
    U=np.zeros((rows,columns))
    P=np.identity(columns)

    for i in range(rows):
        maxindex = np.argmax(np.abs(matrix[i:,i]))
        maxindex = maxindex+i
        if maxindex!=i:
            exchange(matrix, maxindex, i)
            exchange(L,maxindex,i)
            exchange(P,maxindex,i)
        L[i][i]=1
        for row in range(i+1, rows):
            param = matrix[row][i]/matrix[i][i]
            L[row][i]=param
            for j in range(i+1, columns):
                matrix[row][j]=(-param)*matrix[i][j]+matrix[row][j]
    for i in range(rows):
        for j in range(i,columns):
            U[i][j]=matrix[i][j]
    return P,L,U

'''
QR分解函数
'''
def QRDecompose(matrix):
    rows,columns = np.shape(matrix)
    Q = np.zeros((rows, columns))
    R = np.zeros((rows, columns))

    for k in range(rows):
        x = matrix[:,k]
        x_copy = matrix[:,k]
        for j in range(k+1):
            if j==k:
                R[j][k]=np.linalg.norm(x)
                Q[:,k]=x / R[j][k]
            else:
                R[j][k]=np.dot(Q[:,j],x_copy)
                x = x - R[j][k]*Q[:,j]
    return R,Q

'''
Household
'''
def Household(matrix):
    rows,columns = np.shape(matrix)
    R=[]

    for i in range(rows-1):
        A=matrix[i:rows,i:rows]
        R.append(np.identity(rows))
        e1=np.zeros((1,rows-i))
        e1[0][0]=1
        u1=A[:,0]-np.linalg.norm(A[:,0])*e1
        u1_T=np.transpose(u1)
        R_sub=np.identity(rows-i)-2*(np.dot(u1_T,u1))/np.dot(u1,u1_T)
        R[i][i:rows,i:rows]=R_sub
        matrix[i:rows,i:rows]=np.around(np.dot(R_sub,A),decimals=3)

    P=np.identity(rows)
    for j in range(len(R)):
        P = np.dot(R[j], P)
    
    return matrix,P

'''
Givens
'''
def Givens(matrix):
    rows,columns = np.shape(matrix)
    P=[[[] for _ in range(rows)] for _ in range(rows)]
    Q=np.identity(rows)

    for i in range(0,rows-1):
        for j in range(i+1, rows):
            col=matrix[:,i]
            x = col[i]
            y = col[j]
            cos=x/np.sqrt(x**2+y**2)
            sin=y/np.sqrt(x**2+y**2)
            P[i][j]=np.identity(3)
            P[i][j][i,j]=sin
            P[i][j][j,i]=-sin
            P[i][j][i,i]=cos
            P[i][j][j,j]=cos
            matrix=np.around(np.dot(P[i][j],matrix),decimals=3)
            Q=np.dot(P[i][j], Q)
    return matrix,Q

if __name__=="__main__":
    print("Welcome to Decompose program")
    print("Input the Number of the Decompose")
    print("1.PLU  2.QR  3.Household  4.Givens 5.quit program")
    print("By the way,if you wanna to change original matrix, you need to alter it in the python file")
    while True:
        number=input("Input Number:")
        number=int(number)

        if number==1:
            matrix = np.array([[1,2,-3,4],
                        [4,8,12,-8],
                        [2,3,2,1],
                        [-3,-1,1,-4]], dtype=float)
            print("original PLU matrix:")
            print(matrix)
            P,L,U=LUDecompose(matrix)
            print("P:")
            print(P)
            print("L:")
            print(L)
            print("U:")
            print(U)
        elif number==2:
            matrix = np.array([[0,-20,-14],
                        [3,27,-4],
                        [4,11,-2]], dtype=float)
            R, Q = QRDecompose(matrix)
            print("origin QR matrix:")
            print(matrix)
            print("Q:")
            print(Q)
            print("R:")
            print(R)
        elif number==3:
            matrix = np.array([[0,-20,-14],
                        [3,27,-4],
                        [4,11,-2]], dtype=float)
        
            print("origin Household matrix:")
            print(matrix)
            R,Q=Household(matrix)
            print("R:")
            print(R)
            print("Q:")
            print(Q)
        elif number==4:
            matrix = np.array([[0,-20,-14],
                        [3,27,-4],
                        [4,11,-2]], dtype=float)
            print("origin Givens matrix:")
            print(matrix)
            R,Q=Givens(matrix)
            print("R:")
            print(R)
            print("Q:")
            print(Q)
        elif number==5:
            print("GoodBye")
            break
        else:
            print("illegal input")
        print("\n")
```
