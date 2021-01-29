

# AsyncTask
AsyncTask是一个轻量级的异步任务类，它可以在线程池中执行后台任务，然后把执行的进度和结果传递给主线程并且在主线程中更新UI。

## AsyncTask类定义
```kotlin

class MyAsyncTask: AsyncTask<Unit, Int, Boolean>() {

}
```
第一个参数：Params 在执行AsyncTask时需要传入的参数，以便于在后台任务使用
第二个参数：Progress 在后台执行任务时，需要显示进度的参数
第三个参数：Result 当任务执行完毕后，如果需要对结果进行返回，则使用这里的参数。

Async需要重载的方法：

方法1， onPreExecute()，在主线程中执行，任务开启前的准备工作；

方法2，doInbackground(Params…params)，开启子线程执行后台任务；

方法3，onProgressUpdate(Progress values)，在主线程中执行，更新UI进度；

方法4，onPostExecute(Result result)，在主线程中执行，异步任务执行完成后执行，它的参数是doInbackground()的返回值。

方法5，onCancelled ()，在调用AsyncTask的cancel()方法时调用

## AsyncTask的调用

MyAsyncTask asyncTask = new MyAsyncTask(); 

asyncTask.execute("http://www.google.com");/这里的参数会被传入到doInbackground中去。


## AsyncTask的取消

asyncTask.cancel()


## 一个例子
```java
 class MyAsyncTask extends AsyncTask<String, Integer, String> 
	    { 
	        /** 
	         * 异步任务：AsyncTask<Params, Progress, Result> 
	         * 1.Params:UI线程传过来的参数。 
	         * 2.Progress:发布进度的类型。 
	         * 3.Result:返回结果的类型。耗时操作doInBackground的返回结果传给执行之后的参数类型。 
	         * 
	         * 执行流程： 
	         * 1.onPreExecute() 
	         * 2.doInBackground()-->onProgressUpdate() 
	         * 3.onPostExecute() 
	         */
  			@Override
	        protected void onPreExecute()//执行耗时操作之前处理UI线程事件 
	        { 
	            progressBar.setVisibility(View.VISIBLE);//点击之后，下载执行之前，设置进度条可见 
	        } 
  
	        @Override
	        protected String doInBackground(String...  params)//执行耗时操作 
	        { 
	            //在此方法执行耗时操作,耗时操作中发布进度，更新进度条 
	            //String result = download(); 
	            for (int i = 0; i < 100; i++) 
	            { 
	                try
	                { 
	                    Thread.sleep(100); 
	                    publishProgress(i);//进度条每次更新10%,执行中创建新线程处理onProgressUpdate()
        
	                } 
	                catch (InterruptedException e) 
	                { 
	                    e.printStackTrace(); 
	                } 
	                    
	            } 
	            return "下载完成!"; 
	        } 

   			@Override                      
	        protected void onProgressUpdate(Integer... values)//执行操作中，发布进度后 
	        { 
	            progressBar.setProgress(values[0]);//每次更新进度条 
	            String str = Integer.toString(values[0]);
	            textView.setText(str); 
	        } 
	            
	        @Override
	        protected void onPostExecute(String result)//执行耗时操作之后处理UI线程事件 
	        { 
	            //在此方法执行main线程操作 
	            progressBar.setVisibility(View.GONE);//下载完成后，隐藏进度条 
	            textView.setText(result); 
	        } 
	            
	    } 
	}	   

MyAsyncTask().execute(200)
MyAsyncTask().executeOnExecutor(AsyncTask.THREAD_POOL_EXECUTOR, "");
```
