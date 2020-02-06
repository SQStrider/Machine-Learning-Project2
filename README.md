# CarND-LeNet-Lab
LeNet5由7层CNN（不包含输入层）组成，输入的原始图像大小是32×32像素.

![LeNet-5 Architecture](lenet.png)
Implement the LeNet-5 deep neural network model.

### 1、C1层（卷积层）：6@28×28
    该层使用了6个卷积核，每个卷积核的大小为5×5，这样就得到了6个feature map（特征图）。
#### （1）特征图大小
    每个卷积核（5×5）与原始的输入图像（32×32）进行卷积，这样得到的feature map（特征图）大小为（32-5+1）×（32-5+1）= 28×28
#### （2）参数个数
    由于参数（权值）共享的原因，对于同个卷积核每个神经元均使用相同的参数，因此，参数个数为（5×5+1）×6= 156，其中5×5为卷积核参数，1为偏置参数
#### （3）连接数
卷积后的图像大小为28×28，因此每个特征图有28×28个神经元，每个卷积核参数为（5×5+1）×6，因此，该层的连接数为（5×5+1）×6×28×28=122304
### 2、S2层（池化层）：6@14×14
#### （1）特征图大小
    这一层主要是做池化或者特征映射（特征降维），池化单元为2×2，因此，6个特征图的大小经池化后即变为14×14。回顾本文刚开始讲到的池化操作，池化单元之间没有重叠，在池化区域内进行聚合统计后得到新的特征值，因此经2×2池化后，每两行两列重新算出一个特征值出来，相当于图像大小减半，因此卷积后的28×28图像经2×2池化后就变为14×14。
#### （2）参数个数
    S2层由于每个特征图都共享相同的w和b这两个参数，因此需要2×6=12个参数
### （3）连接数
    下采样之后的图像大小为14×14，因此S2层的每个特征图有14×14个神经元，每个池化单元连接数为2×2+1（1为偏置量），因此，该层的连接数为（2×2+1）×14×14×6 = 5880
### 3、C3层（卷积层）：16@10×10
    C3层有16个卷积核，卷积模板大小为5×5。
#### （1）特征图大小
    与C1层的分析类似，C3层的特征图大小为（14-5+1）×（14-5+1）= 10×10
#### （2）参数个数
    C3层的参数数目为（5×5×3+1）×6 +（5×5×4+1）×9 +5×5×6+1 = 1516
#### （3）连接数
    卷积后的特征图大小为10×10，参数数量为1516，因此连接数为1516×10×10= 151600
### 4、S4（下采样层，也称池化层）：16@5×5
#### （1）特征图大小
   与S2的分析类似，池化单元大小为2×2，因此，该层与C3一样共有16个特征图，每个特征图的大小为5×5。
#### （2）参数数量
    与S2的计算类似，所需要参数个数为16×2 = 32
#### （3）连接数
    连接数为（2×2+1）×5×5×16 = 2000
### 5、C5层（卷积层）：120
#### （1）特征图大小
    该层有120个卷积核，每个卷积核的大小仍为5×5，因此有120个特征图。由于S4层的大小为5×5，而该层的卷积核大小也是5×5，因此特征图大小为（5-5+1）×（5-5+1）= 1×1。这样该层就刚好变成了全连接，这只是巧合，如果原始输入的图像比较大，则该层就不是全连接了。
#### （2）参数个数
    与前面的分析类似，本层的参数数目为120×（5×5×16+1） = 48120
#### （3）连接数
    由于该层的特征图大小刚好为1×1，因此连接数为48120×1×1=48120
###6、F6层（全连接层）：84
#### （1）特征图大小
    F6层有84个单元，之所以选这个数字的原因是来自于输出层的设计，对应于一个7×12的比特图
    该层有84个特征图，特征图大小与C5一样都是1×1，与C5层全连接。
### （2）参数个数
    由于是全连接，参数数量为（120+1）×84=10164。跟经典神经网络一样，F6层计算输入向量和权重向量之间的点积，再加上一个偏置，然后将其传递给sigmoid函数得出结果。
#### （3）连接数
    由于是全连接，连接数与参数数量一样，也是10164。
### 7、OUTPUT层（输出层）：10
    Output层也是全连接层，共有10个节点，分别代表数字0到9。如果第i个节点的值为0，则表示网络识别的结果是数字i。
#### （1）特征图大小
    该层采用径向基函数（RBF）的网络连接方式，假设x是上一层的输入，y是RBF的输出，则RBF输出的计算方式是：上式中的Wij的值由i的比特图编码确定，i从0到9，j取值从0到7×12-1。RBF输出的值越接近于0，表示当前网络输入的识别结果与字符i越接近。
#### （2）参数个数
    由于是全连接，参数个数为84×10=840
#### （3）连接数
    由于是全连接，连接数与参数个数一样，也是840

