---
title: linear algebra homework
tags: homework
comment: true
date: 2019-10-08 12:09:00
---
write a lu decompose program

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
