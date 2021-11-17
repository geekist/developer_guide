
# Masonry 介绍


## 一、常用的属性与常量

### 1. MASViewAttribute 以对应的系统类型


|MASViewAttribute	| NSLayoutAttribute |
|:----------|:----------|
|view.mas_left	|NSLayoutAttributeLeft
|view.mas_right	|NSLayoutAttributeRight
|view.mas_top	|NSLayoutAttributeTop
|view.mas_bottom	|NSLayoutAttributeBottom
|view.mas_leading	|NSLayoutAttributeLeading
|view.mas_trailing	|NSLayoutAttributeTrailing
|view.mas_width	|NSLayoutAttributeWidth
|view.mas_height	|NSLayoutAttributeHeight
|view.mas_centerX	|NSLayoutAttributeCenterX
|view.mas_centerY	|NSLayoutAttributeCenterY
|view.mas_baseline	|NSLayoutAttributeBaseline


### 2.UIView


先来一波最为常用的使用方法，大家可以看一下大致语法，下面会细讲使用

```c
//.分别设置各个相对边距（superview为view的父类视图，下同）

make.left.mas_equalTo(superView.mas_left).mas_offset(10);

make.right.mas_equalTo(superView.mas_right).mas_offset(-10);

make.top.mas_equalTo(superView.mas_top).mas_offset(10);

make.bottom.mas_equalTo(superView.mas_bottom).offset(-10);

```

```c
//直接连接使用left大于等于每个值
make.left.mas_greaterThanOrEqualTo(10);
```

```c
//设置宽和高

make.width.mas_equalTo(60);

make.height.mas_equalTo(60);
```

```c
//.设置center和款高比

make.center.mas_equalTo(superView);

make.width.mas_equalTo(superView).multipliedBy(1.00/3);

make.height.mas_equalTo(superView).multipliedBy(0.25);
```

```c
//.关于约束优先级,此处要注意约束冲突的问题，统一约束优先级大的生效

make.left.mas_equalTo(100);

make.left.mas_equalTo(view.superview.mas_left).offset(10);

make.left.mas_equalTo(20).priority(700);

make.left.mas_equalTo(40).priorityHigh();

make.left.mas_equalTo(60).priorityMedium();

make.left.mas_equalTo(80).priorityLow();
```

```
//.如果你想让view的（x坐标）左边大于等于label的左边，以下两个约束的写法效果一样

 make.left.greaterThanOrEqualTo(label);

 make.left.greaterThanOrEqualTo(label.mas_left);

注：约束的链式写法中，不包含其他相对的view时，默认为其superview，即make.left.mas_equalTo(100);等价于
make.left.mas_equalTo(view.superview.mas_left).offset(10);和

make.left.mas_equalTo(view.superview).offset(10);
```

### 3. NSNumber

自动布局允许使用常量去设置宽或高，如果你想通过一个数字设置一个view的最小和最大的width，可以用equality blocks，如下：

```c
//width >= 200 && width <= 400

make.width.greaterThanOrEqualTo(@200);

make.width.lessThanOrEqualTo(@400)

```
然而自动布局不允许对齐属性的约束（如：left,right,centerY等）设置为常量值，你可以使用NSNumber来设置相对于父类view这些约束属性，如：

```c
// creates view.left = view.superview.left + 10

make.left.lessThanOrEqualTo(@10)

```
如果你不想使用NSNumber来设置，也可以用如下结构来创建你的约束，如：

```c
make.top.mas_equalTo(42);

make.height.mas_equalTo(20);

make.size.mas_equalTo(CGSizeMake(50, 100));

make.edges.mas_equalTo(UIEdgeInsetsMake(10, 0, 10, 0));

make.left.mas_equalTo(view).mas_offset(UIEdgeInsetsMake(10, 0, 10, 0));
```

以上用法默认添加mas_前缀，如果你不想添加此前缀，但还想使用常量值设置约束，需要在导入Masonry头文件前，设置宏定义MAS_SHORTHAND_GLOBALS，至于为什么，去masonry代码中搜索一下MAS_SHORTHAND_GLOBALS便知。

### 4. NSArray


用数组添加集中不同类的约束，如：

```
make.height.equalTo(@[view1.mas_height, view2.mas_height]);

make.height.equalTo(@[view1, view2]);

make.left.equalTo(@[view1, @100, view3.right]);

```

## 二、约束的优先级属性

.priority允许你设置一个非常准确的的约束优先级（0-1000）

.priorityHigh 相当于系统的 UILayoutPriorityDefaultHigh

.priorityMedium 介于 high and low之间的优先级

.priorityLow 相当于系统的 UILayoutPriorityDefaultLow

注：默认通过mas_make添加的约束不设置优先级时，默认都是最高（1000）

优先级属性可以放在约束链的末端使用，如：

```
make.left.greaterThanOrEqualTo(label.mas_left).with.priorityLow();

make.top.equalTo(label.mas_top).with.priority(600);
```
## 三、更加便利的约束方法

Masonry提供了一些便利的方法供我们同时创建多个不同的约束，他们被称为MASCompositeConstraints，如：

```c
edges
// 使一个view的top, left, bottom, right 等于view2的

make.edges.equalTo(view2)；
//相对于superviewde上左下右边距分别为5，10，15，20

make.edges.equalTo(superview).insets(UIEdgeInsetsMake(5, 10, 15, 20))
size
// 使得宽度和高度大于等于 titleLabel

make.size.greaterThanOrEqualTo(titleLabel)
// 相对于superview宽度大100，高度小50

make.size.equalTo(superview).sizeOffset(CGSizeMake(100, -50))
center
//中心与button1对齐

make.center.equalTo(button1)
//水平方向中心相对向左偏移5，竖直方向中心向下偏移10

make.center.equalTo(superview).centerOffset(CGPointMake(-5, 10))
你可以在约束链里添加相应的view来增加代码的可读性:

// 除了top，所有的边界与superview对齐

make.left.right.and.bottom.equalTo(superview);

make.top.equalTo(otherView);

```

## 四、关于如何修改约束

有时候，你为了实现动画或者移除替换一些约束时，你需要去修改一些已经存在的约束，Masonry提供了一些不同的方法去更新约束，你也可以将多个约束存在数组里

1. References

你可以持有某个特定的约束，让其成为成员变量或者属性

```c
//设置为公共或私接口

@property (nonatomic, strong) MASConstraint *topConstraint;

...

// 添加约束

[view1 mas_makeConstraints:^(MASConstraintMaker *make) {

self.topConstraint = make.top.equalTo(superview.mas_top).with.offset(padding.top);

make.left.equalTo(superview.mas_left).with.offset(padding.left);

}];

...

// 然后可以调用
//该约束移除

[self.topConstraint uninstall];

//重新设置value,最常用

self.topConstraint.mas_equalTo(20);

//该约束失效
[self.topConstraint deactivate];

//该约束生效
[self.topConstraint activate];

```
2. mas_updateConstraints

如果你只是想更新一下view对应的约束，可以使用 mas_updateConstraints 方法代替 mas_makeConstraints方法

```c
//这是苹果推荐的添加或者更新约束的地方

// 在响应setNeedsUpdateConstraints方法时，这个方法会被调用多次

// 此方法会被UIKit内部调用，或者在你触发约束更新时调用

- (void)updateConstraints {

[self.growingButton mas_updateConstraints:^(MASConstraintMaker *make) {

make.center.equalTo(self);

make.width.equalTo(@(self.buttonSize.width)).priorityLow();

make.height.equalTo(@(self.buttonSize.height)).priorityLow();

make.width.lessThanOrEqualTo(self);

make.height.lessThanOrEqualTo(self);

}];

//调用super
[super updateConstraints];

}

```
3. mas_remakeConstraints


mas_updateConstraints只是去更新一些约束，然而有些时候修改一些约束值是没用的，这时候mas_remakeConstraints就可以派上用场了

mas_remakeConstraints某些程度相似于mas_updateConstraints，但不同于mas_updateConstraints去更新约束值，他会移除之前的view的所有约束，然后再去添加约束

```c
- (void)changeButtonPosition {

      [self.button mas_remakeConstraints:^(MASConstraintMaker *make) {

      make.size.equalTo(self.buttonSize);

     if (topLeft) {

          make.top.and.left.offset(10);

     } else {

     make.bottom.and.right.offset(-10); 

   }

  }];

```

## 五、在哪创建我的约束

贴一个官方说明的例子：

```c
@implementation DIYCustomView

- (id)init {

self = [super init];

if (!self) return nil;

// --- Create your views here ---

self.button = [[UIButton alloc] init];

return self;

}

// tell UIKit that you are using AutoLayout

+ (BOOL)requiresConstraintBasedLayout {

return YES;

}

// this is Apple's recommended place for adding/updating constraints

- (void)updateConstraints {

// --- remake/update constraints here

[self.button remakeConstraints:^(MASConstraintMaker *make) {

make.width.equalTo(@(self.buttonSize.width));

make.height.equalTo(@(self.buttonSize.height));

}];

//according to apple super should be called at end of method

[super updateConstraints];

}

- (void)didTapButton:(UIButton *)button {

// --- Do your changes ie change variables that affect your layout etc ---

self.buttonSize = CGSize(200, 200);

// tell constraints they need updating

[self setNeedsUpdateConstraints];

}

```

## 六、Layout必备知识

AutoLayout关于更新的几个方法的区别

setNeedsLayout：告知页面需要更新，但是不会立刻开始更新。执行后会立刻调用layoutSubviews。

layoutIfNeeded：告知页面布局立刻更新。所以一般都会和setNeedsLayout一起使用。如果希望立刻生成新的frame需要调用此方法，利用这点一般布局动画可以在更新布局后直接使用这个方法让动画生效。

layoutSubviews：系统重写布局

setNeedsUpdateConstraints：告知需要更新约束，但是不会立刻开始

updateConstraintsIfNeeded：告知立刻更新约束

updateConstraints：系统更新约束

## 七、使用tips

* 给view添加约束时，必须已经添加到其superview上面

* 不需要设置view.translatesAutoresizingMaskIntoConstraints = NO;，masonry内部已经帮我设置过了

* 手写布局时，合理使用约束，尽量约束冲突问题

* 因为iOS中原点在左上角所以注意使用offset时注意right和bottom用负数

