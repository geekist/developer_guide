

# Audio--用mediaPlayer播放音频

## 初始化
```kotlin
 private val mediaPlayer = MediaPlayer()

 private fun initMediaPlayer() {
        val assetManager = assets;
        val fd = assetManager.openFd("music.mp3")
        mediaPlayer.setDataSource(fd.fileDescriptor,fd.startOffset,fd.length)
        mediaPlayer.prepare()
    }

```

## 释放资源
```java
  override fun onDestroy() {
        super.onDestroy()
        mediaPlayer.stop()
        mediaPlayer.release()
    }

```


## 开始播放
```java
 buttonPlay.setOnClickListener{
            if(!mediaPlayer.isPlaying) {
                mediaPlayer.start()
            }
        }

```


## 暂停播放
```java
  buttonPause.setOnClickListener{
            if(mediaPlayer.isPlaying) {
                mediaPlayer.pause()
            }
        }


```

## 停止播放

```java
  buttonstop.setOnClickListener{
            if(mediaPlayer.isPlaying) {
                mediaPlayer.reset()
                initMediaPlayer()
            }
        }

```

