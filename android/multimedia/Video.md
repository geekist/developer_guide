

# 用VideoView播放视频


## 初始化
```kotlin
   val uri = Uri.parse("android.resource://$packageName/${R.raw.video}")
        videoView.setVideoURI(uri)
```

## 释放资源
```java
   override fun onDestroy() {
        super.onDestroy()
        videoView.suspend()
    }

```


## 开始播放
```java
  buttonPlayVideo.setOnClickListener{
            if(!videoView.isPlaying) {
                videoView.start()
            }
        }
```


## 暂停播放
```java
  buttonPauseVideo.setOnClickListener{
            if(videoView.isPlaying) {
                videoView.pause()
            }
        }


```

## replay

```java
    buttonReplayVideo.setOnClickListener{
            if(videoView.isPlaying) {
                videoView.resume()
            }
        }

```

tiDex 支持库。