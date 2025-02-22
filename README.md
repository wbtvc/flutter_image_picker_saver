# forked自https://github.com/cnhefang/image_picker_saver，修复一些bug
目前我只用到保存到相册功能，发现原版以下bug:
1. iOS：当保存图像到相册没有相册权限时，原版居然没有没有返回result给flutter，导致flutter中的await永远等待，真是太愚蠢了。
Android：因为我不熟悉android开发，而且没有android测试机，暂时还不知道有没问题。


# Flutter Image Picker and Saver plugin for Flutter

  Android supported

  IOS supported 8.0+

forked from official plugin image_picker and add saver function to save image to photo gallery.

## Installation
```
  flutter_image_picker_saver:
    git: 
      url: "https://github.com/wbtvc/flutter_image_picker_saver.git"
```
 
### Save image Example
``` dart

    void _onImageSaveButtonPressed() async {
      print("_onImageSaveButtonPressed");
      var response = await http
          .get('http://upload.art.ifeng.com/2017/0425/1493105660290.jpg');
  
      debugPrint(response.statusCode.toString());
  
      var filePath = await ImagePickerSaver.saveFile(
          fileData: response.bodyBytes);
  
      var savedFile= File.fromUri(Uri.file(filePath));
      setState(() {
        _imageFile = Future<File>.sync(() => savedFile);
      });
    }

```

### iOS

Add the following keys to your _Info.plist_ file, located in `<project root>/ios/Runner/Info.plist`:

* `NSPhotoLibraryUsageDescription` - describe why your app needs permission for the photo library. This is called _Privacy - Photo Library Usage Description_ in the visual editor.
* `NSCameraUsageDescription` - describe why your app needs access to the camera. This is called _Privacy - Camera Usage Description_ in the visual editor.
* `NSMicrophoneUsageDescription` - describe why your app needs access to the microphone, if you intend to record videos. This is called _Privacy - Microphone Usage Description_ in the visual editor.

### Android

No configuration required - the plugin should work out of the box.

### Example

``` dart
import 'package:image_picker/image_picker.dart';

class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => new _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  File _image;

  Future getImage() async {
    var image = await ImagePicker.pickImage(source: ImageSource.camera);

    setState(() {
      _image = image;
    });
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Image Picker Example'),
      ),
      body: new Center(
        child: _image == null
            ? new Text('No image selected.')
            : new Image.file(_image),
      ),
      floatingActionButton: new FloatingActionButton(
        onPressed: getImage,
        tooltip: 'Pick Image',
        child: new Icon(Icons.add_a_photo),
      ),
    );
  }
}
```
