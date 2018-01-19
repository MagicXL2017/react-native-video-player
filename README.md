## 更新内容说明

### 2018-01-19
解决在华为荣耀机型4.4.4  api19上不支持COLOR_FormatYUV420Planar问题


### 2018-01-17
修改安卓端API19时，闪退问题，19改回COLOR_FormatYUV420Planar编码
isSupportPlanarYUV增加版本判断

### 2018-01-15
按官方方法，修改IOS端通过style属性来控制KSYVideo，取消videoframe属性

### 2018-01-14 

- 修改安卓端文件保存目录为：
```
Environment.getExternalStorageDirectory() + "yunkan/screenshots"
Environment.getExternalStorageDirectory() + "yunkan/records"
Environment.getExternalStorageDirectory() + "yunkan/recordScreenshots"
```
- 修改安卓端返回uri的值
> file:/storage/emulated/0/yunkan/screenshots/20180115-012220-109.png

- 关于安卓端目录创建权限问题

> 保持项目的targetSdkVersion 22，当targetSdkVersion>=23时，安卓需要以显示方式单独申请操作权限，此问题以后再处理

- 修改安卓端isSupportPlanarYUV文件，判断是否为COLOR_FormatYUV420Flexible，解决绿屏问题

### 2018-01-11 新增加视频录像首页截图功能功能

- 录像返回时，只在开始返回值中包括录像缩略图参数

recordScreenshotPath
recordScreenshotURL

- 例子中，VideoList修改原filePath为全局RootPath

> let recordScreenshot = "file://"+RootPath+"/recordScreenshots/"+item.name+".png";

recordScreenshot文件名格式为：xxxxxx.mp4.png

- ios中，目前取网关中rtmp流录像保存时出错，此音频采样为8000，目前调整为小于44100的默认为44100

- 取消example中的KSYVideo.js文件，防止运行时，把组件重复注册

### 2018-01-10 新增加视频截图和录像功能
* Android 取图片目录

Environment.getExternalStorageDirectory() + "DCIM/video"

* Android 取视频目录
Environment.getExternalStorageDirectory() + "DCIM/image"

* IOS 取图片目录
DocumentPath + "/screenshots"

* IOS 取视频目录
DocumentPath + "/records"

* 取图方法 *
> 使用react-native-fs库进行取图操作，注意，需要对取出的列表进行倒序显示
> 取文件时，在android上需要在路径前加"file://"



## react-native-video-player
<pre>Source Type:<b> Open Source</b></pre>


> Media Player for React-Native, base on KSYMediaPlayer.

More details about KSYMediaPlayer, please click the link blow:

* [KSYMediaPlayer Android SDK for vod or live streaming playing][player_android]
* [KSYMediaPlayer iOS SDK for vod or live streaming playing][player_ios]

*Android support*

*iOS support*


### 1. About

A `<KSYVideo>` component for react-native,requires react-native >= 0.49.0

#### 1.1 Add it to your project
* Install via npm

Run `npm install react-native-ksyvideo --save`


### 1.2 Usage Example

```javascript

<KSYVideo source={{uri: "rtmp://"}}   // Can be a URL or a local file.
       ref={(ref) => {
         this.player = ref
       }}                                      // Store reference
  
       volume={1.0}                            
       muted={false}                           
       paused={false}                          // Pauses playback entirely.
       resizeMode="cover"                      // Fill the whole screen at aspect ratio.*
       repeat={true}                           // Repeat forever.
       playInBackground={false}                // Audio continues to play when app entering background.
       progressUpdateInterval={250.0}          // Interval to fire onProgress (default to ~250ms)
       onLoadStart={this.loadStart}            // Callback when video starts to load
       onLoad={this.setDuration}               // Callback when video loads
       onProgress={this.setTime}               // Callback every ~250ms with currentTime
       onEnd={this.onEnd}                      // Callback when playback finishes
       onError={this.videoError}               // Callback when video cannot be loaded
       onBuffer={this.onBuffer}                // Callback when remote video is buffering
       style={styles.backgroundVideo} />


// Later on in your styles..
var styles = StyleSheet.create({
  backgroundVideo: {
    position: 'absolute',
    top: 0,
    left: 0,
    bottom: 0,
    right: 0,
  },
});
```

### 2. Android Integration

Run `react-native link` to link the react-native-ksyvideo library.

Or if you have trouble, make the following additions to the given files manually:

**android/settings.gradle**

```gradle
include ':react-native-ksyvideo'
project(':react-native-ksyvideo').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-ksyvideo/android')
```

**android/app/build.gradle**

```gradle
dependencies {
   ...
   compile project(':react-native-ksyvideo')
}
```

**MainApplication.java**

On top, where imports are:

```java
import com.ksyun.media.reactnative.ReactKSYVideoPackage;
```

Add the `ReactKSYVideoPackage` class to your list of exported packages.

```java
@Override
protected List<ReactPackage> getPackages() {
    return Arrays.asList(
            new MainReactPackage(),
            new ReactKSYVideoPackage()
    );
}
```

### 3. iOS Integration

Run `react-native link` to link the react-native-ksyvideo library.

Add KSYMediaPlayer.framework(which is in node_modules/react-native-ksyvideo/ios directory) to your project setting 'target->Build Phases->Link Binary With Libraries'.

Add Framework Search Path : '../node_modules/react-native-ksyvideo/ios'


### 4. Remarks
If you want to updata native sdk for KSYVideo,make the following additions
#### 4.1 Android
The KSYVideo is dependented on [jcenter](https://bintray.com/ksvc/ksyplayer),you can modify the dependencies ,update to high version

build.gradle(Module:react-native-ksyvideo)
```gradle
dependencies {

    compile "com.facebook.react:react-native:+"  // From node_modules
    compile 'com.ksyun.media:libksyplayer-arm64:2.1.0'
    compile 'com.ksyun.media:libksyplayer-x86:2.1.0'
    compile 'com.ksyun.media:libksyplayer-armv7a:2.1.0'
    compile 'com.ksyun.media:libksyplayer-java:2.1.0'
}
```


#### 4.2 iOS

Get the latest framework of KSYMeidaPlayer_iOS at https://github.com/ksvc/KSYMediaPlayer_iOS/releases, then replace the old framework in node_modules/react-native-ksyvideo/ios directory

### 5. LICENSE
[Apache 2.0](LICENSE)

[player_android]:https://github.com/ksvc/KSYMediaPlayer_Android
[player_ios]:https://github.com/ksvc/KSYMediaPlayer_iOS

