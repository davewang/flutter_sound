# flutter_sound
This is not a playlist audio module and this library provides simple recorder and player functionalities for both `android` and `ios` platforms. This only supports default file extension for each platform. This module can also handle file from url.

## Getting Started

For help getting started with Flutter, view our online
[documentation](https://flutter.io/).

## Install
Add ```flutter_sound``` as a dependency in pubspec.yaml
For help on adding as a dependency, view the [documentation](https://flutter.io/using-packages/).

## Post Installation
On *iOS* you need to add a usage description to `info.plist`:

```xml
<key>NSMicrophoneUsageDescription</key>
<string>This sample uses the microphone to record your speech and convert it to text.</string>
<key>UIBackgroundModes</key>
<array>
	<string>audio</string>
</array>
```

On *Android* you need to add a permission to `AndroidManifest.xml`:
```xml
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

## Methods
| Func  | Param  | Return | Description |
| :------------ |:---------------:| :---------------:| :-----|
| setSubscriptionDuration | `double sec` | `String` message | Set subscription timer in seconds. Default is `0.01` if not using this method.|
| startRecorder | `String uri` | `String` uri | Start recording. This will return uri used. |
| stopRecorder | | `String` message | Stop recording.  |
| startPlayer | `String uri` | `String` message | Start playing.  |
| stopPlayer | | `String` message | Stop playing. |
| pausePlayer | | `String` message | Pause playing. |
| resumePlayer | | `String` message | Resume playing. |
| seekToPlayer | `int sec` position to goTo | `String` message | Seek audio to selected position in seconds. Parameter should be less than audio duration to correctly placed. |

## Subscriptions
| Subscription | Return | Description |
| onRecorderStateChanged | `<RecordStatus>` | Able to listen to subscription when recorder starts. |
| onPlayerStateChanged | `<PlayStatus>` | Able to listen to subscription when player starts. |


## Usage
#### Creating instance.
```dart
FlutterSound flutterSound = new FlutterSound();
```

#### Starting recorder with listener.
```dart
String path = await flutterSound.startPlayer(null);
print('startPlayer: $path');
_playerSubscription = flutterSound.onPlayerStateChanged.listen((e) {
	if (e != null) {
		DateTime date = new DateTime.fromMillisecondsSinceEpoch(e.currentPosition.toInt());
		String txt = DateFormat('mm:ss:SS', 'en_US').format(date);
		this.setState(() {
			this._isPlaying = true;
			this._playerTxt = txt.substring(0, 8);
		});
	}
});
```

#### Stop recorder
```dart
String result = await flutterSound.stopRecorder();
print('stopRecorder: $result');

if (_recorderSubscription != null) {
	_recorderSubscription.cancel();
	_recorderSubscription = null;
}
```

#### Start player
```dart
String path = await flutterSound.startPlayer(null);
print('startPlayer: $path');

_playerSubscription = flutterSound.onPlayerStateChanged.listen((e) {
	if (e != null) {
		DateTime date = new DateTime.fromMillisecondsSinceEpoch(e.currentPosition.toInt());
		String txt = DateFormat('mm:ss:SS', 'en_US').format(date);
		this.setState(() {
			this._isPlaying = true;
			this._playerTxt = txt.substring(0, 8);
		});
	}
});
```

#### Stop player
```dart
String result = await flutterSound.stopPlayer();
print('stopPlayer: $result');
if (_playerSubscription != null) {
	_playerSubscription.cancel();
	_playerSubscription = null;
}
```

#### Pause player
```dart
String result = await flutterSound.pausePlayer();
```

### Resume player
```dart
String result = await flutterSound.resumePlayer();
```

#### Seek player
```dart
String result = await flutterSound.seekToPlayer(sec);
```

#### Setting subscription duration (Optional). 0.01 is default value when not set.
```dart
/// 0.01 is default
flutterSound.setSubscriptionDuration(0.01);
```