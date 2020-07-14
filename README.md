# wear_mask_to_face
该代码基于开源库dlib，face，face_recognition

对原始人脸图片戴口罩后并对齐、裁剪成128*128大小的图片
支持数据集输入，输出

该算法由四个模块组成，分别是：主体模块、人脸关键点检测模块、口罩佩戴模块和人脸对齐模块。软件的系统总框图下图所示。

![Image text](https://github.com/Amoswish/wear_mask_to_face/blob/master/img/fig1.png)


## 主体模块
主体模块进行了相关参数初始化、数据的读取，以及各部分模块的调用。说明如下：

### 参数说明

    face_locations = [left_up,left_down,right_up,right_down] # 人脸在图片中的位置

    face_landmarks = []#人脸68个关键点坐标

    CROPE_SIZE = 128 #最后输出的人脸图片尺寸

    predictor = dlib.shape_predictor("./models/shape_predictor_68_face_landmarks.dat") #预训练模型位置

    unmasked_paths=[] #记录未成功佩戴口罩的位置
### 数据的读取

    dataset_path =' ' 输入图片文件夹的位置

    output_dir = ‘’ 输出文件位置

### 调用流程

 ![Image text](https://github.com/Amoswish/wear_mask_to_face/blob/master/img/fig2.png)


此模块的流程如下：

（1）初始化相关参数值：face_locations，face_landmarks，CROPE_SIZE = 128，
predictor

（2）读入数据；

（3) 调用人脸坐标点检测模块、口罩佩戴模块和人脸对齐模块。 

（4) 保存佩戴完口罩的图片，并记录佩戴失败的图片

## 人脸关键点检测模块
本模块用于计算人脸关键点位置坐标，并返回输入图片face_locations，face_landmarks。

### 方法说明
（1）	face_recognition.face_locations（face_img）方法。
    获得输入图片中人脸位置的坐标。其基本参数如下：

    img（以numpy读取的图片）

	number_of_times_to_upsample	（寻找人脸时对图片上采样的次数，数字越大，越可以找到更小的人脸，默认为1）

	model （使用哪种面部检测模型。“ hog”不太准确，但在CPU上更快。“ cnn”是经过GPU / CUDA加速（如果可用）的更准确的深度学习模型。 默认为“hog）

（2）face_recognition.face_landmarks(face_image_np, face_locations)方法，计算人脸关键点，返回face_landmarks。其基本参数如下：

    face_image_np	（需要检测人脸关键点的图片）

    face_locations	（可选）提供面部位置列表以进行检查
    model	可选-使用哪种型号。 “大”（默认）或“小”，仅返回5点，但速度更快

人脸关键点检测的处理流程图如下图所示。

 ![Image text](https://github.com/Amoswish/wear_mask_to_face/blob/master/img/fig3.png)

## 口罩佩戴模块
本模块的作用是根据检测出的人脸关键点，将口罩图片放缩成合适大小并放到合适位置。
### 方法说明
add_mask方法是该模块的主要方法，其基本参数如下：
 
    mask_path	口罩图片位置
     
	face_locations	人脸位置坐标矩阵

	face_landmarks	人脸关键点坐标矩阵

get_distance_from_point_to_line方法，获得点到线距离，其基本参数如下：
 
    get_distance_from_point_to_line	point	点的坐标，由长度为2的一维数组构成。
 
	line_point1	线的第一个端点坐标，由长度为2的一维数组构成。
     
	line_point2	线的第二个端点坐标，由长度为2的一维数组构成。

FaceMasker类：对人脸图片加入口罩

口罩佩戴的流程图如下图所示。

 ![Image text](https://github.com/Amoswish/wear_mask_to_face/blob/master/img/fig4.png)

## 人脸对齐模块
当人脸图片佩戴好口罩后，为提高人脸识别精度，需要将图片进行旋转和重塑尺寸。
### 方法说明
face_alignment方法是该模块的主要方法，其基本参数如下：

    face_alignment	faces	戴完口罩的人脸数据矩阵

    shape_predictor	预训练dlib人脸关键点检测模型

人脸对齐模块处理流程如下：

 ![Image text](https://github.com/Amoswish/wear_mask_to_face/blob/master/img/fig5.png)

lfw数据集 效果图
![Image text](https://github.com/Amoswish/wear_mask_to_face/blob/master/img/1.jpg)
![Image text](https://github.com/Amoswish/wear_mask_to_face/blob/master/img/2.jpg)
