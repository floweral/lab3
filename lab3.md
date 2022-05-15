# 一、下载源码

本次实验的代码可以直接到github的TensorFlow项目中直接下载，也可以在git上进行clone

git clone的命令为打开要下载的文件位置，开启Git 

代码下载后结果为![p1](https://github.com/floweral/images/blob/lab3/p2.png)

```
git clone https://github.com/hoitab/TFLClassify.git
```



# 二、导入下载的模型

### 1、导入代码配置相关环境

在编译项目过程中，会弹出"Gradle Sync"，应该选择"OK",下载相应的gradle wrapper 。

配置后在Android Studio的项目架构如下

![p2](https://github.com/floweral/images/blob/lab3/p2.png)



### 2、连接安卓真机

本次实验需要真实的摄像头进行测试，所以应该采用真机进行测试

连接后的配置结果如下：

![p3](https://github.com/floweral/images/blob/lab3/p6.png)



### 3、添加TensorFlow Lite模块

- 右键“start”模块，或者选择File，然后New>Other>TensorFlow Lite Mode

- 选择下载了的训练模型![p7](https://github.com/floweral/images/blob/lab3/p7.png)

- 成功导入结果如下

  ![p4](https://github.com/floweral/images/blob/lab3/p3.png)



### 4、在代码的TODO选项，通过操作修改代码

- View>Tool Windows>TODO

- 选择Group By结果如图![p5](https://github.com/floweral/images/blob/lab3/p8.png)

- 修改代码

  - 定位TODO 1，添加初始化训练模型的代码

  ```kotlin
    // TODO 1: Add class variable TensorFlow Lite Model 
    private val flowerModel = FlowerModel.newInstance(ctx)
  ```

  - 在CameraX的analyze方法内部，需要将摄像头的输入`ImageProxy`转化为`Bitmap`对象，并进一步转化为`TensorImage` 对象

  ```kotlin
    // TODO 2: Convert Image to Bitmap then to TensorImage
    val tfImage = TensorImage.fromBitmap(toBitmap(imageProxy))
  ```

  - TODO3

  ```kotlin
  override fun analyze(imageProxy: ImageProxy) {
    ...
    // TODO 3: Process the image using the trained model, sort and pick out the top results
    val outputs = flowerModel.process(tfImage)
        .probabilityAsCategoryList.apply {
            sortByDescending { it.score } // Sort with highest confidence first
        }.take(MAX_RESULT_DISPLAY) // take the top results
  
    ...
  }
  ```

  - TODO4

  ```kotlin
  override fun analyze(imageProxy: ImageProxy) {
    ...
    // TODO 4: Converting the top probability items into a list of recognitions
    for (output in outputs) {
        items.add(Recognition(output.label, output.score))
    }
    ...
  }
  ```

  - 将原先用于虚拟显示识别结果的代码注释掉或者删除

  ```kotlin
  for (i in 0..MAX_RESULT_DISPLAY-1){
      items.add(Recognition("Fake label $i", Random.nextFloat()))
  }
  ```



# 三、运行

最终在真机上运行项目，选择一张图片进行测试

结果如下

![p6](https://github.com/floweral/images/blob/lab3/p5.png)
