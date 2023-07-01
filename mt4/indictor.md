## 技术指标概念

技术指标分为两类：系统内置的技术指标和自定义技术指标

## 系统内置的技术指标

### 内置指标的概念

内置的技术指标，其代码不可进行修改。 因此，用户可以避免内置技术指标的错误修改。 但是，计算技术指标的源代码可在软件开发人员网站 (MetaQuotes Ltd.) 的技术指标部分中找到。 如果需要，程序员可以使用完整代码或部分代码来创建自定义指标（请参阅创建自定义指标）。


用户可见的图形表示由客户端显示。

* 指标线
  
指标线是基于指标数组中包含的数值的某种依赖性的图形显示。

指标线类型由用户设置。 指示线可以以实线或虚线、指定颜色的形式显示，也可以以某些符号（点、方块、环等）链的形式显示。 在指标计算过程中，会在其中计算一组数值； 将根据这些计算绘制指标线。 这些值集存储在指标数组中。


* 指标数组
  
指标数组是一个包含数值的一维数组，根据该数值构建指标线。 指标数组元素的数值是点坐标，在其上绘制指标线。 每个点的 Y 坐标是指标数组元素的值，X 坐标是指标数组元素的索引值。

指标数组中的数据存储技术是构建技术指标和自定义指标的基础。 技术指标的指标数组元素的值可从所有应用程序中获取，包括 EA 交易、脚本和自定义指标。 为了在应用程序中获取具有特定索引的指标数组元素的值，需要调用内置函数，该函数的名称根据技术指标名称来设置。

为了执行技术指标功能，相应的指标不必附加到安全窗口。 此外，来自应用程序的技术指标函数调用不会导致将相应的指标附加到安全窗口。 将技术指标附加到安全窗口也不会导致应用程序中的技术指标调用。

### 调用系统内置指标

#### iMA移动平均线指标

移动平均线 (MA) 显示特定时间段内工具价格的平均值。 该指标反映了总体市场趋势 - 可以增加、减少或在某个价格附近显示一些波动。

移动平均线函数定义如下：

```c
double iMA(string symbol, int timeframe, int period, int ma_shift, int ma_method, int applied_price, int shift)


Parameters:

symbol  货币对的名称，函数将根据其数据计算指标。 如果该参数为空：NULL 表示当前货币对。

timeframe  时间范围。 可以是图表周期之一。 0 表示当前图表的周期。

period  MA 计算的平均周期。

ma_shift  相对于价格图表的指标偏移。

ma_method  平均方法。 可以是 MA 方法值之一。

applied_price 使用价格。 可以是任何价格常数。

shift  从指标数组获取的值索引（相对于当前柱向后移动指定数量的柱）。

```

调用移动平均线指标的一个EA

```c
//+------------------------------------------------------------------+
//|                                             CallMAIndicatior.mq4 |
//|                                  Copyright 2023, MetaQuotes Ltd. |
//|                                             https://www.mql5.com |
//+------------------------------------------------------------------+
#property copyright "Copyright 2023, MetaQuotes Ltd."
#property link      "https://www.mql5.com"
#property version   "1.00"
#property strict

extern int Period_MA = 21;            // Calculated MA period
bool Fact_Up = true;                  // Fact of report that price..
bool Fact_Dn = true;                  //..is above or below MA
//+------------------------------------------------------------------+
//| Expert initialization function                                   |
//+------------------------------------------------------------------+
int OnInit()
  {
//--- create timer
   EventSetTimer(60);

//---
   return(INIT_SUCCEEDED);
  }
//+------------------------------------------------------------------+
//| Expert deinitialization function                                 |
//+------------------------------------------------------------------+
void OnDeinit(const int reason)
  {
//--- destroy timer
   EventKillTimer();

  }
//+------------------------------------------------------------------+
//| Expert tick function                                             |
//+------------------------------------------------------------------+
void OnTick()
  {
//---
   double MA;                         // MA value on 0 bar
//--------------------------------------------------------------------
// Tech. ind. function call
   MA=iMA(NULL,0,Period_MA,0,MODE_SMA,PRICE_CLOSE,0);
//--------------------------------------------------------------------

   Print("current Bid is: ", Bid, " ---MA value is : ",MA);
   if(Bid > MA && Fact_Up == true)    // Checking if price above
     {
      Fact_Dn = true;                 // Report about price above MA
      Fact_Up = false;                // Don't report about price below MA
      Alert("Price is above MA(",Period_MA,").");// Alert
     }
//--------------------------------------------------------------------
   if(Bid < MA && Fact_Dn == true)    // Checking if price below
     {
      Fact_Up = true;                 // Report about price below MA
      Fact_Dn = false;                // Don't report about price above MA
      Alert("Price is below MA(",Period_MA,").");// Alert
     }
//--------------------------------------------------------------------
   return;                            // Exit start()

  }
//+------------------------------------------------------------------+
//| Timer function                                                   |
//+------------------------------------------------------------------+
void OnTimer()
  {
//---

  }
//+------------------------------------------------------------------+


```

说明：

该EA在每个价格变动时，监控价格是否穿越了移动平均线，并在向上或向下穿越移动平均线的k线收盘时，弹出Alert窗口。

#### iStochastic随机震荡指标

随机震荡指标将当前收盘价与选定时间段的价格范围进行比较。 该指标通常由两条指标线表示。 
主要的一个称为%K。 第二条 %D 信号线是 %K 的移动平均线。 通常 %K 绘制为实线，%D - 虚线。 

根据指标解释变体之一，如果 %K 高于 %D，我们应该买入；如果 %K 低于 %D，我们应该卖出。 执行交易操作的最有利时刻被认为是线路一致的时刻。

```c
double iStochastic( string symbol, int timeframe, int %Kperiod, int %Dperiod, int slowing, int method, int price_field, int mode, int shift) 

计算随机震荡指标并返回它的值。 

参量:

symbol   -   计算指标数据上的货币对名称. NULL表示当前货币对. 

timeframe   -   时间周期。 可以时间周期列举任意值. 0表示当前图表的时间周期. 

%Kperiod   -   %K 周期线。. 

%Dperiod   -   %D周期线. 

slowing   -   滚动值. 

method   -   MA方法。 它可以是其中任意 滑动平均值列举 值. 

price_field   -   价格参量.可以是以下值: 0 - Low/High 或者 1 - Close/Close. 

mode   -   指标行数组索引。它可以是 指标识别符列举的任意值. 

shift   -   从显示缓冲采取的值的索引(转移相对当前柱特定相当数量期间前). 
```

示例:

  
```c
int start()                       // Special function start()
  {
   double M_0, M_1,               // Value MAIN on 0 and 1st bars
          S_0, S_1;               // Value SIGNAL on 0 and 1st bars
//--------------------------------------------------------------------
                                  // Tech. ind. function call
   M_0 = iStochastic(NULL,0,5,3,3,MODE_SMA,0,MODE_MAIN,  0);// 0 bar
   M_1 = iStochastic(NULL,0,5,3,3,MODE_SMA,0,MODE_MAIN,  1);// 1st bar
   S_0 = iStochastic(NULL,0,5,3,3,MODE_SMA,0,MODE_SIGNAL,0);// 0 bar
   S_1 = iStochastic(NULL,0,5,3,3,MODE_SMA,0,MODE_SIGNAL,1);// 1st bar
//--------------------------------------------------------------------
                                  // Analysis of the situation
   if( M_1 < S_1 && M_0 >= S_0 )  // Green line crosses red upwards
      Alert("Crossing upwards. BUY."); // Alert 
   if( M_1 > S_1 && M_0 <= S_0 )  // Green line crosses red downwards
      Alert("Crossing downwards. SELL."); // Alert 
      
   if( M_1 > S_1 && M_0 > S_0 )   // Green line higher than red
      Alert("Continue holding Buy position.");       // Alert 
   if( M_1 < S_1 && M_0 < S_0 )   // Green line lower than red
      Alert("Continue holding Buy position.");       // Alert 
//--------------------------------------------------------------------
   return;                         // Exit start()
  }

```



## 自定义指标