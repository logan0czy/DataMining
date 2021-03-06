# About The Ways to Get Optimal Parameters in Linear Regression
- The Normal Equation
- [Gradient Descent](#GDs)

## The Normal Equation
1.根据损失函数(MSE)在训练集上最小化求得参数解析解，复杂度为O(n^2.4)-O(n^3); 2.直接用训练集矩阵的伪逆乘上label向量得到参数值，复杂度为O(n^2).  
两种方法对训练集大小复杂度都为O(m).

<span id='GDs'></span>
## Gradient Descent
**Warning**:梯度下降之前，确保每一个特征的值域都在相同尺度上`例如使用Scikit-Learn's StandardScaler class`,否则目标函数在不同维度下降速率不同，如在值大的维度下降的快，在值小的维度下降的慢，参数收敛到最优值会受影响.  
| | Batch GD | Stochastic GD | Mini-batch GD |
|:----:|:--------:|:-------------:|:-------------:|
| Data Size | Full | One instance | Partial |
| Speed | Slow | Fast | Medium |
| Variation | Weak | Strong | Medium |  
> 解决迭代次数设置问题：设置过小可能还没到最优点，过大会浪费训练时间。办法是设置较大的迭代次数，同时设置一个容错量epsilon，当梯度二范数小于该值时停止迭代。  

> 退火方法(simulated annealing)：随着迭代进行，为了稳定在最优点附近，不断降低学习率.  

# Underfitting and Overfitting  
- [Polynomial Regression](#PolyRegress)  
- [Learning Curves](#Curves)
- [Regularization](#Reg)
  - Ridge Regression
  - Lasso Regression  

<span id='PolyRegress'></span>
## Polynomial Regression  
当模型非线性时，通过添加多项式特征进行拟合--`sklearn.peprocessing.PolynomialFeatures`. 不过特征个数会急剧增长 ~ (n+d)!/(d!n!).  

<span id='Curves'></span>
## Learning Curves  
模型性能随着训练集变大在训练集和验证集上的表现曲线 —— **相比于交叉验证，通过学习曲线更能看出模型过拟合/欠拟合是否因为模型本身，而不是数据集不够。**  
- Underfitting: train&val曲线相互靠近在较高水平，此时增加训练集对提升模型性能没有帮助  
- Overfitting: train曲线在较低水平，val在较高水平，train-val间最终有差距，增加训练集可以缩小两者之间的差距  

|  | Underfitting | Overfitting |
|:----:|:--------:|:--------:|
| Bias | high | low |
| Variance | low | high |
| Irreducible error | - | - |  
> The only way to reduce the irreducible error is to clean up the data (e.g., fix the data sources, such as broken data, detect and remove outliers).

<span id='Reg'></span>
## Regularization
> A good training cost function should have regularization and optimization-friendly derivitives, while the performance measure used for testing should be as close as possible to the final objectives.  

| | Ridge | Lasso |
|:----:|:----:|:----:|
| Form | Square of L2 norm | L1 norm |  

- Lasso: Least Absolute Shrinkage and **Selection Operator** Regression. 当权重不为0时，正则项对应导数不为0，且与权重值无关，为+1/-1。Lasso回归天然得具有`特征选择`的特性，它会将不重要的特征置0，只保留相对重要的特征，因而最终输出的是一个`稀疏模型`.  
- Ridge: 正则项的导数与对应权重相关，因而在梯度下降时，权重大的梯度大，权重小的梯度小，Ridge回归会保留所有特征项，只是将每一个特征的权重都更新地比较小。  

**Elastic Net**   
A middle ground between Ridge and Lasso with the mix ratio r (scikit-learn `sklearn.linear_model.ElasticNet`).  
Priority: Ridge > Elastic Net > Lasso. 当特征数量大于训练数据量时，或一些特征有较强相关性时，Lasso回归不是很稳定。  

**Early Stopping**  
模型在验证集上的性能随着训练的进行会先降后升，后半段意味着模型出现了过拟合，因此需要在训练中途保存最优模型并适时停止，方便迭代完成后模型回滚到最优点。  

# Logistic Regression  
- `Softmax Regression`: or Multinomial Logistic Regression. Make logistic be a multiclass classifier.  
- `Loss/Cost Function`: **cross entropy**, measure the degree of difference between two distribution.  
- `Regularization term` in scikit-learn: C. The higher, the less the model is regularized.  
