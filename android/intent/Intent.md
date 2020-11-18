
# Intent

##  Start Activity

* 打电话

```java
  try {
            val intent = Intent(Intent.ACTION_CALL)
            intent.data = Uri.parse("tel:10086")
            startActivity(intent)
        }catch (e: SecurityException) {
            e.printStackTrace()
        }

```