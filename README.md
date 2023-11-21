# 2023.19
1.实现思路
  首先读取路径下的文本文件，获得文本和对应的标签，导出为csv文件
  dataset读取csv文件，将从路径获得的文本编码处理成input_ids,attention_mask,input_type_ids,labels
  dataloader定义collate_fn获取一个batchsize的数据，padding填充到相同长度，squeeze整理成正确的形状，labels转化为long类型数据
  实例化dataloader，导入bert-base-uncased
  将获取的数据集分为test集和train集
  定义model任务处理模型，嵌入pretrained，设置输出层fc的长度
  定义损失函数，优化函数，实例化模型，在train集上进行训练
  最后在test集上进行训练
2.遇到的bug
  直接从文本路径获取的数据长短不一，需要padding处理成相同长度
  encoding出来的张量形状需要修改为模型对应的张量形状
  pretrained要提前嵌入到model模型内部，不然会报错
  从huggingface网站上导入的bert-base-uncased模型会导入到cpu上，训练速度太慢，需要手动调节到gpu上
  读取出来的label是float类型的，要在collate_fn中修改为long类型、
  collate_fn不能返回字典格式的数据，要直接返回张量形式
3.其他
  了解了绘制roc和auc曲线的过程
      创建字典，首先获取每个样本对应不同类型的最大概率值存入字典，导入sklearn库，调用函数计算TPR，FPR存入字典，绘制图像（二分类，多分类）
  从所有的编码层内抽取嵌入表示的方法
  模型结果争取率比较低，目前看还需要进一步提高学习率，但是时间成本有点高，目前只调到了85%左右
  
#2023.11.21
绘制roc曲线，计算auc分析模型性能
