## AlertDialog

```java

    override fun onClick(v: View?) {
        when(v?.id) {
            R.id.textview_first ->{Toast.makeText(context,"interface clicked",Toast.LENGTH_LONG).show()}
            R.id.button_first ->{
                AlertDialog.Builder(context).apply {
                    setTitle("this is a dialog")
                    setMessage("this is a message")
                    setCancelable(false)
                    setPositiveButton("OK"){dialog,which->}
                    setNegativeButton("Cancel"){dialog,which->run{
                        Toast.makeText(context,"this is from dialog",Toast.LENGTH_LONG).show()
                    }}
                    show()
                }
            }

        }
    }


```