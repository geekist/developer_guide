
# ios产品发布的基本概念

苹果为了控制应用的开发与发布流程，制定了一套非常复杂的机制。这里面的关键词有：个人开发者账号，企业开发者账号，bundle ID，开发证书，发布证书（又叫“生产证书”），开发描述文件，发布描述文件，推送证书等…。初级开发者很难理解这中间的关系和作用。下文就对上述列出的关键内容和它们的关系做个详细介绍。


## 开发者账号


开发者账号其实就是apple id，只不过要成为开发者，需要去苹果开发者中心激活开发权限。

激活开发者账号分为三种，个人，公司和企业。这三种账号各自有些区别，下表是他们主要区别的对比：


| 账号类型  |支持applestroe上架   |  测试设备数量  | 申请费用  | 申请费用  |
| -------- | ------------       | ------------- | ------  | ------  |
|个人|	是|	100|	99$|	－|
|公司|	是	|100	|99$	|邓氏码|
|企业|	否	|不限	|299$	|邓氏码|

所以如果应用需要上appstroe，就要创建个人或者公司类型的账号；

如果只想内部发布，就可以申请企业账号；

个人账号和公司账号的区别就是公司账号能邀请多个账号，他们可以和你共享资源协同开发此应用，而不需要总是问你要你的账号使用。

##  bundle ID  --- APP ID

bundle ID可以翻译成包ID,也可以叫APP ID 或应用ID,它是每一个ios应用的全球唯一标识。

无论代码怎么改，图标和应用名称怎么换，只要bundle id没变，ios系统就认为这是同一个应用。每开发一个新应用，首先都需要到member center->identifier->APP IDS去创建一个bundle id。

bundle id分为两种：

Explicit App ID（明确的APP ID），一般格式是：com.domainname.appname。这种id只能用在一个app上。每一个新应用都要创建一个。

Wildcard App ID(通配符APP ID), 一般格式是：com.domainname.*。这种id可以用在多个应用上，虽然方便，但是使用这种id的应用不能使用通知功能，所以并不常用。

![](../assets/yuya_account.png)

##  iOS Certificates ---证书

ios certificates就是证书。它的作用就是证明你的mac具有开发或发布某个开发者账号下应用的权限。

证书还分成两种，一种是开发证书，也叫Development certificate; 另一种是发布证书或叫生产证书，英文名叫Production certificate。

* development certificate

开发证书是证明你的mac具有开发和真机调试你的程序的东西。***每个证书关联着一个开发者账号***，也就是说，你只能使用这个证书开发这个账号底下的应用，它是通过bundle id来知道该应用是不是属于自己的账号。所以，每次为一个开发者帐号开发应用，就得需要这个账号创建的开发证书。


*Production certificate

生产证书是证明你的mac是否具有发布应用的权限。在archive到appstore的时候，必须有生产证书，否则时上传不成功的。

* 根证书

无论是开发证书还是生产证书，都必须从根证书上派生。根证书就是根mac产生的证书，member center上保存的那个证书就是根证书，它只能安装在创建它的那台mac上，其他的开发mac必须从那台根mac上导出证书使用。也就是所有的子证书都只能从根证书派生而来。


## iOS Provisioning Profiles ---描述文件

iOS Provisioning Profiles也叫描述文件，它用来标识某个设备是否具有安装某个应用的权限。描述文件分为开发描述文件和发布描述文件。其中发布描述文件又分为临时发布描述文件和正式发布描述文件。

* 开发描述文件(develop provisioning profile)

开发描述文件，创建它需要bundle ID + 开发证书 ＋ 测试设备(在devices中添加); 编译的时候必须三者对应，否则编译器会报错。


* 临时发布描述文件(distribution ad hoc)


临时发布描述文件是用来在有限设备上发布的。创建它需要bundle ID ＋发布证书＋发布设备，编译时也必须三者对应。

* 正式发布描述文件(distribution appstore/distribution ad house)

个人账号和企业账号创建的正式发布描述文件是不同的。因为个人账号是在appstrore上发布，所以创建的是distribution appstore描述文件，而企业账号只能内部发布，所以创建的叫distribution ad house描述文件。但是它们创建所需要的东西都是一样的：bundle ID + 发布证书。 因为他们是在不确定的设备上安装的，所以不需要选择设备。







