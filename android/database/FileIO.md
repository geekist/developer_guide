# FileIO

适合存储简单的数据或二进制文件，文件存储在data/data/packagename/files目录下

## 将数据存储到文件中
```kotlin
 private fun save(strData: String) {
        try{
            val output = openFileOutput("0701_file", MODE_PRIVATE)
            val writer = BufferedWriter(OutputStreamWriter(output))
            writer.use {
                it.write(strData)
            }
        }catch(e: IOException) {
            e.printStackTrace()
        }
    }

```

## 从文件中存储数据
```kotlin
  private fun load(): String {
        val content = StringBuilder()
        try{
            val input = openFileInput("0701_file")
            val reader = BufferedReader(InputStreamReader(input))
            reader.use {
                reader.forEachLine {
                    content.append(it)
                }
            }
        }catch(e: IOException) {
            e.printStackTrace()
        }
        return content.toString()
    }

```

