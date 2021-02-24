### dart学习
#### const、final的区别
const可以开始不赋值，只能赋一次；final不仅有const的编译时常量的特性，最重要它是运行时常量，并且final是惰性初始化，即在运行时第一次使用前才初始化。
### dart中的库
- 自定义的库：import 'path/xxx.dart'
- 系统内置的库
- Pub包管理中的库: https://pub.dev/packages
#### 部分导入
导入需要的部分，使用show关键字
```dart
import 'path/packageName' show functionName;
```
隐藏不需要的部分，使用hide关键字
```dart
import 'path/packageName' hide functionName;
```

### 打包
需要在不同平台打包

### MAC版环境安装
1. 从appStore中获取Xcode
2. 安装flutter SDK（https://flutter.dev/）

### 创建项目相关
创建项目: flutter create 项目名称

#### 修改项目文件夹权限【和flutterSDK文件夹】
进入到文件夹同层目录修改权限：chmod -R 777 文件夹名称（ 或者进入文件夹内部运行：chmod -R 777 * ）

### vscode连接模拟器
#### 手动打Xcode
如果vscode终端底部bar显示 No Devices,则把模拟器中flutter应用删除, vscode底部显示连接设备；在终端运行 flutter run（如果运行时连接卡死，网络修改连接热点）    
#### 终端打开
在终端输入命令：open -a Simulator    

### vscode终端快捷键
- r Hot reload.  
- o 切换预览模式ios和Android
- p 显示/隐藏网格 
- R Hot restart.    
- h Repeat this help message.    
- d Detach (terminate "flutter run" but leave application running).   
- c Clear the screen   
- q Quit (terminate the application on the device).   

### 基础
- 所有组件都是类
- dart/flutter实例化类的时候可以省略new 关键字
- flutter中所有和数字相关的都是double类型
- {} 表示可选命名参数
  
#### MaterialApp组件
一般作为顶层Widget使用    
##### 常用属性
- home: Scaffold
- title
- color
- theme
- routes

#### Scaffold组件
##### 常用属性
- appBar：显示在界面顶部的一个AppBar
- body：当前界面所显示的主要内容Widget
- drawer：抽屉菜单控件

#### Container组件
```dart
Container({
    key,
    alignment,  // Alignment
    padding,  // EdgeInsets
    color,
    decoration, // BoxDecoration(color, borderRadius, image: image: DecorationImage(), ...)
    foregroundDecoration,
    double width,
    double height,
    BoxConstraints constraints,
    margin,
    transform,  // Matrix4
    child,
    clipBehavior = Clip.none,
  })
```
#### 居中组件
```dart
Center(
  { 
    Key key, 
    double widthFactor, 
    double heightFactor, 
    Widget child 
  }
)  
****
```
#### 文本组件
```dart
Text(
  data, // 必填参数
  { // 可选参数
    key,
    style, // TextStyle() fontSize,color: Color,
    strutStyle,
    textAlign,  // TextAlign
    textDirection,
    locale,
    softWrap,
    overflow, // TextOverflow
    textScaleFactor,
    maxLines,
    semanticsLabel,
    textWidthBasis,
    textHeightBehavior,
  }
)
```
#### 图片组件
```dart
// 网络图片1
child: Image.network( // 常用
  'https://ss0.baidu.com/7Po3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/9c16fdfaaf51f3de9ba8ee1194eef01f3a2979a8.jpg',
  alignment: Alignment.bottomRight,
  color: Colors.yellow,
  colorBlendMode: BlendMode.screen,
  fit: BoxFit.cover,
  repeat: ImageRepeat.repeat,
),
// 网络图片2
image: DecorationImage(
   image: NetworkImage("https://ss0.baidu.com/7Po3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/9c16fdfaaf51f3de9ba8ee1194eef01f3a2979a8.jpg"),
  fit: BoxFit.cover
)

// 本地图片
child: Image.asset( // 常用
  'https://ss0.baidu.com/7Po3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/9c16fdfaaf51f3de9ba8ee1194eef01f3a2979a8.jpg',
  alignment: Alignment.bottomRight,
  color: Colors.yellow,
  colorBlendMode: BlendMode.screen,
  fit: BoxFit.cover,
  repeat: ImageRepeat.repeat,
),
```
##### 本地图片
1. 本地图片需要在文件夹中必须添加2.0x、3.0x文件夹(也可添加4.0x文件夹)    
2. 在pubspec.yaml文件夹中配置本地图片路径

#### ListView组件