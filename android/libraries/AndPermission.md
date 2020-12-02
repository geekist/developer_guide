# AndPermission

AndPermission是一个权限管理框架，包括但不仅限于运行时权限，覆盖绘制权限，显示通知权限和访问通知权限等

## github地址： https://github.com/yanzhenjie/AndPermission

## 依赖项：

implementation 'com.yanzhenjie:permission:2.0.3'

## 请求运行时权限

AndPermission可以使用Context/Fragment请求权限：
```java
AndPermission.with(context)
    .runtime()
    .permission()
    ...

AndPermission.with(fragment)
    .runtime()
    .permission()
    ...
```
基本的Api
请求单个权限：
```java
AndPermission.with(this)
    .runtime()
    .permission(Permission.WRITE_EXTERNAL_STORAGE)
    .onGranted(new Action() {
        @Override
        public void onAction(List<String> permissions) {
            // TODO what to do.
        }
    }).onDenied(new Action() {
        @Override
        public void onAction(List<String> permissions) {
            // TODO what to do
        }
    })
    .start();
```
请求单个权限组：
```java
AndPermission.with(this)
    .runtime()
    .permission(Permission.Group.STORAGE)
    ...
请求多个权限：

AndPermission.with(this)
    .runtime()
    .permission(
        Permission.WRITE_EXTERNAL_STORAGE,
        Permission.CAMERA
    )
    ...

```
请求多个权限组：

```java
AndPermission.with(this)
    .runtime()
    .permission(
        Permission.Group.STORAGE,
        Permission.Group.CAMERA
    )
    ...

```
当被拒绝时
用户往往会拒绝一些权限，而程序的继续运行又必须使用这些权限，此时我们应该友善的提示用户。

例如，当用户拒绝Permission.WRITE_EXTERNAL_STORAGE一次，在下次请求Permission.WRITE_EXTERNAL_STORAGE时，我们应该展示为什么需要此权限的说明，以便用户判断是否需要授权给我们。在AndPermission中我们可以使用Rationale。

```java
private Rationale mRationale = new Rationale() {
    @Override
    public void showRationale(Context context, List<String> permissions, 
    RequestExecutor executor) {
        // 这里使用一个Dialog询问用户是否继续授权。

        // 如果用户继续：
        executor.execute();

        // 如果用户中断：
        executor.cancel();
    }
};

AndPermission.with(this)
    .runtime()
    .permission(Permission.WRITE_EXTERNAL_STORAGE)
    .rationale(mRationale)
    .onGranted(...)
    .onDenied(...)
    .start();
```
当总是被拒绝
当用户点击应用程序的某个按钮，而他又总是拒绝我们需要的某个权限时，应用程序可能不会响应（但不是ANR），为了避免这种情况，我们应该在用户总是拒绝某个权限时提示用户去系统设置中授权哪些权限给我们，无论用户是否真的会授权给我们。

```java
AndPermission.with(this)
    .runtime()
    .permission(...)
    .rationale(...)
    .onGranted(...)
    .onDenied(new Action() {
        @Override
        public void onAction(List<String> permissions) {
            if (AndPermission.hasAlwaysDeniedPermission(context, permissions)) {
            // 这些权限被用户总是拒绝。
            }
        }
    })
    .start();
```
接下来我们应该提示用户去系统设置中授权这些权限给我们。

```java
private static final int REQ_CODE_PERMISSION = 1;
...

if (AndPermission.hasAlwaysDeniedPermission(context, permissions)) {
    // 用Dialog展示没有某权限，询问用户是否去设置中授权。
    AndPermission.with(this)
        .runtime()
        .setting()
        .start(REQ_CODE_PERMISSION);
}

...

@Override
protected void onActivityResult(int reqCode, int resCode, Intent data) {
    switch (reqCode) {
        case REQ_CODE_PERMISSION: {
            if (AndPermission.hasPermissions(this, ...)) {
                // 有对应的权限
            } else {
                // 没有对应的权限
            }
            break;
        }
    }
}
```

当然，我们需要提示用户应该授权哪些权限，转换为文字详情请移步到权限常量。

特别注意：AndPermission.hasAlwaysDeniedPermission()只能在onDenied()的回调中调用，不能在其它地方使用。

友情提示
上述做法是标准做法，但是部分中国产手机，由于系统原因，不会调用rationale()的回调方法，如果用户多次拒绝某一权限，那么该权限可能每次申请都会直接调用onDenied()的回调方法，因此建议直接在onDenied()的回调方法中提示用户没有该权限，让用户直接去设置中开启权限，然后走设置的逻辑即可。

