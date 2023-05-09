```js
class MyApp extends StatelessWidget {
  const MyApp({super.key});
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
    	title: 'fultter',
      home: Scaffold(
      	appBar: AppBar(
      		title: const Text('Flutter'),
    		)
        body: const Center(
          	child: Text('hello world'),
        )
    	)
    )
  }
}
```

```dart
Row(
	mainAxisAlignment: MainAxisAlugnment.spaceEvenly,
  children: [
  	Image.asset('img1.png'),
  	Image.asset('img2.png'),
  	Image.asset('img3.png'),
  ]
)

Column(
	mainAxisAlignment: MainAxisAlignment.spaceEvenly,
  children: [
  	Image,asset('img1.png'),
  	Image,asset('img2.png'),
  	Image,asset('img3.png'),
  ]
)

var stars = Row(
	mainAxisSize: MainAxisSize.min,
  children: [
  	Icon(Icon.star, color: Color.green[500]),
  	Icon(Icon.star, color: Color.green[500]),
  	Icon(Icon.star, color: Color.green[500]),
  	const Icon(Icon.star, color: Color.black),
  	const Icon(Icon.star, color: Color.black),
  ]
)
final ratings = Container(
	padding: const EdgeInsets.all(20),
  child: Row(
  	mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
    	stars,
    	const Text(
    		'170 Reviews',
    		style: TextStyle(
    			color: Color.black,
    			fontWeight: FontWeight.w800,
    			fontFamily: 'Roboto',
    			letterApacing: 0.5,
    			fontSize: 20,
  			)
  		)
    ]
  )
)

const descTextStyle = TextStyle(
	color: Color.black,
  fontWeight: FontWeight.w800,
  fontFmaily: 'Roboto',
  letterSpacing: 0.5,
  fontSize: 30,
  height: 2
)

final iconList = DefaultTextStyle.merge(
	style: descTextStyle,
  child: Container(
		padding: const EdgeInsets.all(20),
  	child: [
      Column(
      	children: [
        	Icon(Icons.kitchen, color: Color.green[500]),
        	const Text('PREP'),
          const Text('25 min'),
        ],
      ),
      Column(
      	children: [
        	Icon(Icon.timer, color: Color.green[500]),
        	const Text('COOK:'),
          const Text('1 hr'),
        ],
      ),
      Column(
      	childen: [
        	Icon(Icons.restaurant, color: Color.green[500]),
        	const Text('FEEDS:'),
          const Text('4-6'),
        ],
      ),
    ],
	),
);



final leftColumn = Container(
	padding: const EdgeInsets.fromLTRB(20, 30, 20, 20),
  child: Column(
  	children: [
    	titleText,
    	subTitle,
    	ratings,
    	iconList,
    ],
  ),
);


body: Center(
	child: Container(
  	margin: const EdgeInsets.fromLTPB(0, 40, 0, 30),
  	height: 600,
    child: Card(
      child: Row(
      	crossAxisAligment: CrossAxisAlignment.start,
      	children: [
      		SizeBox(
      			width: 400,
      			child: leftColumn,
    			),
      		mainImage,
      	],
    	),
    ),
	),
),
  
Widget _buildImageColumn() {
  return Container(
  	decoration: const BoxDecoration(
    	color: Color.black26,
    ),
    child: Column(
    	children: [
        _buildImageRow(1),
        _buildImageRow(3),
      ],
    ),
  );
}

Widget _buildDecoratedImage(int imageIndex) => Expanded(
	child: Container(
  	decoration: BoxDecoration(
    	border: Border.all(width: 10, color: Color.black38),
      borderRadius: const BorderRadius.all(Radius.circular(8)),
    ),
    margin: const EdgeInsets.all(4),
    child: Image.asset('image1.png'),
  ),
);

Widget _buildImageRow(int imageIndex) => Row(
	children: [
    _buildDecoratedImage(imageIndex),
    _buildDecoratedImage(imageIndex + 1),
  ],
);

Widget _buildGrid() => {
  maxCrossAxisExtent: 150,
  padding: const EdgeInsets.all(4),
  mainAxisSoacing: 4,
  crossAxisSpacing: 4,
  children: _buildGridTitlelist(30),
};


widget _buildList() {
  return ListView(
  	children: [
      _title('abc', 'def', Icons.theaters),
      _title('abc', 'def', Icons.theaters),
      _title('abc', 'def', Icons.theaters),
      _title('abc', 'def', Icons.theaters),
      _title('abc', 'def', Icons.theaters),
      _title('abc', 'def', Icons.theaters),
      _title('abc', 'def', Icons.theaters),
    ],
  );
};

List _title(String title, String subtitle, IconData icon) {
  return ListTitle(
  	title: Text(
      title,
      style: const TextStyle(
      	fontWeight: FontWeight.w500,
        fontSize: 20,
      ),
    ),
    subtitle: Text(subtitle),
    leading: Icon(
    	icon,
      color: Color.bule[500],
    ),
  );
};

Widget _buildStack() {
  return Stack(
  	alignment: const Alignment(0.6, 0.6),
    children: [
      const CircleAvatar(
      	backgroundImage: AssetImage('image1.png'),
        radius: 100,
      ),
      Container(
      	decoration: const BoxDecoration(
        	color: Color.black45,
        ),
        child: const Text(
        	'Mia B',
          style: TextStyle(
          	fontSize: 20,
            fontWeight: FontWeight.bold,
            color: Color.white,
          ),
        ),
      ),
    ],
  );
}

Widget _buildCard() {
  return SizeBox(
  	height: 210,
    child: Card(
    	child: Column(
      	children: [
          ListTile(
          	title: const Text(
            	'1625 Main Street',
              style: TextStyle(fontWeight: FontWeight.w500),
            ),
            subtitle: const Text('My City, CA 99984'),
            leading: Icon(
            	Icons.restaurant_menu,
              color: Color.buld[500],
            ),
          ),
          const Divider(),
          ListTile(
          	title: const Text(
            	'(408) 555-1212',
              style: TextStyle(fontWeight: FontWeight.w500),
            ),
            leading: Icon(
            	Icons.contact_phone,
              color: Color.blue[500],
            ),
          ),
          ListTile(
          	title: const Text('costa@example.com'),
            leading: Icon(
            	Icons.contact_mail,
              color: Color.blue[500],
            ),
          ),
        ],
      ),
    ),
  );
}

```



布局构建

```dart
import 'package:flutterr/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});
  
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
    	title: 'laout title',
      home: Scaffold(
      	appBar: AppBar(
        	title: const Text('layout title'),
        ),
        body: const Center(
        	child: Text('hello'),
        ),
      ),
    );
  }
}


widget titleSection = Container(
	padding: const EdgeInsets.all(32),
  child: Row(
  	children: [
      Expanded(
      	child: column(
        	crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Container(
            	padding: const EdgeInsets.only(bottom: 8),
              child: const Text(
              	'Oeschinen Lake Campground',
                style: TextStyle(fontWeight: FontWeight.bold),
              ),
            ),
            Text(
            	'Kandersteg, Switzerland',
              style: TextStyle(
              	color: Color.gray[500],
              ),
            ),
          ],
        ),
      ),
      Icon(
      	Icons.star,
        color: Color.red[500],
      ),
      const Text('41'),
    ],
  ),
);


return MaterialApp(
	title: 'layout',
  home: zScaffold(
  	appBar: AppBar(
    	title: const Text('layout'),
    ),
    body: Column(
    	children: [
        titleSection,
      ],
    ),
  ),
);


class MyApp extends StatelessWidget{
  const MyApp({super.key});
  
  @override
  Widget build(BuildContext context) {
    // ...
  }
  
  Column _buildButtonColumn(Color color, InconData icon, Stroing label) {
    return Column(
    	mainAxisSize: MainAxisSize.min,
      mainAxisAlignment: MainAlignment.center,
      children: [
        Icon(icon, color: color),
        Container: [
          margin: const EdgeInsets.only(top: 8),
          child: TextStyle(
          	label,
            style: TextStyle(
            	fontSize: 12,
              fontWeight: FontWeight.w400,
              color: color,
            ),
          ),
        ],
      ],
    );
  }
}


Color color = Theme.of(context).promaryColor;

Widget buttonSection = Row(
	mainAxisAlignment: MainAxisAlignment.spaceEvenly,
  children: [
  	_buildButtonColumn(color, Icons.call, 'CALL'),  
  	_buildButtonColumn(color, Icons.near_me, 'ROUTE'),  
  	_buildButtonColumn(color, Icons.share, 'SHARE'),  
  ],
);

return MaterialApp(
	title: 'layout',
  home: Scaffold(
  	body: Column(
    	children: [
        titleSection,
        buttonSection,
      ],
    ),
  ),
)
  
Widget textSection = const Padding(
	padding: EdgeInsets.all(32),
  child: Text(
  	'asdf',
    softWrap: true,
  ),
);

return MaterialApp(
	title: 'layout',
  home: Scaffold(
  	chidren: [
      titleSection,
      buttonSection,
      textSection,
    ]
  ),
),

flutter: 
	user-material-design: true,
	assets:
		- image/lake.jpg

body: Column(
	children: [
    Image.asset(
    	'image/lake.jpg',
      width: 600,
      height: 240,
      fit: Boxfit.coverm,
    ),
    titleSection,
    buttonSection,
    textSection,
  ],
),


```

```dart
main(List<String> args) {
  var p = Person.withNameAgeHeight('www', 18, 1.88);
  var p2 = Person.fromMap({
    "name": 'lili',
    "age": 18,
    "height": 1.88
  });
  print(p1);
}
class Person {
  String name;
  int age;
  double height;
  Person(this.name, this.age);
  
  Person.withNameAgeHeight(this.name, this.age, this.height);
  Person.formMap(Map<string, dynamic> map) {
    this.name = map["name"];
    this.age = map["age"];
    this.height = map["height"];
  }
  
  @voerride
  String toString() {
    return "$name $age $height";
  }
}


```

```dart
// 路由
onPressed: () {
  Navigator.of(context).push(
  	MaterialPageRoute(
    	builder: (context) => const SongScreen(song: song),
    ),
  );
},
child: Text(song.name),

// 使用命名路线
@overtide
Widget build(BuildContext context) {
  return MaterialApp(
  	routes: {
      '/': (context) => HomeScreen(),
      '/details': (context) => DetailScreen(),
    },
  );
}

// 使用路由
MaterialApp.router(
	routerConfig: GoRouter(
  	// ...
  )
)

```



```dart
// Text样式文本
Text("hello world", textAligin: TextAlign.left)
Text("hello world",
   maxLines: 1,
   overflow: TextOverFlow.ellipsis
);
Text(
	"hello world",
  textScaleFactor: 1.5,
)

// Text 指定文本显示的样式
Text(
	"hello world",
  style: TextStyle(
		color: Color.blue,
  	fontSize: 18.0,
  	height: 1.2,
  	fontFamily: "Courier",
  	background: Paint()..color=Color.yellow,
  	decoration: TextDecoration.underline,
    decorationStyle: TextDecoration.deshed
	),
);

// Text 文本片段
Text.rich(TextSpan(
	children: [
  	TextSpan(
			text: "Home:"
		),
    TextSpan(
			text: "http://flitter.dev",
      style: TextStyle(
      	color: Color.blue
      ),
      recognizer: _tapRecgnizer
		)
  ]
));

// DefaultTextStyle 文本默认样式
DefailtTextStyle(
	style: TextStyle(
  	color: Color.red,
    fontSize: 20
  ),
  textAlign: TextAlign.start,
  child: Column(
  	crossAxisAlignment: CrossAxisAlignment.start,
    children: <Widget>[
      Text("hello world"),
      Text("I ma Jack"),
      Text("I am Jack",
         style: TextStyle(
         	inherit: false,
           color: Color.gray
         ),
      ),
    ],
  ),
);

// Button "漂浮"按钮
ElevatedButton(
	child: Text("normal"),
  onPressed: () {},
);

TextButton(
	child: Text("normal"),
  onPressed: () {},
);

OutlineButton(
	child: Text('normal'),
  onPressed: () {},
)
  
IconButton(
	icon: Icon(Icons,thumb_up),
  onPressed: () {},
)
  
ElevatedButton.icon(
	icon: Icon(Icons.send),
  label: Text("发送"),
  onPressed: _onPressed,
),
OutlineButton.icon(
	icon:Icon(Icons.add),
  label: Text("添加"),
  onPressed: _onPressed,
),
TextButton.icon(
	icon: Icon(Icons.info),
  label: Text("详情"),
  onPressed: _onPressed,
),

// 从 asset 中加在图片
Image(
	image: AssetImage("images/a.png"),
  width: 100.0
);

Image.asset(
  "images/a.png",
  width: 100.0
)
  
// 从网络加在数据
Image(
	image: NetworkImage(
  	"http://image.png",
  ),
  width: 100.0
)
  
Image.notWork(
	"https://image.png",
  width: 100.0
)
  
// image 参数
const Image({
  this.width,
  this.height,
  this.color,
  this.colorBlendMode,
  this.fit,
  this.alignment = Alignment.center,
  this.repeat = ImageRepeat.onRepeat,
})

// switch 开关
Switch(
	value: true,
  onChage: (value) {},
);

// checkbox 复选框
Checkout(
	value: true,
  activeColor: Color.red,
  onChange: (value) {},
);

// TextField 文本输入框
TextField(
	autofocus: true,
  onChange:(v){
    print("onChange: $v");
  }
);

// LinearProgressIndicator 线性、条状的进度条
LinearProgressIndicator(
	backgroundColor: Color.gray[200],
  valueColor: AlwaysStoppedAnimation(Colors.blue),
);

LineearProgressIndicator(
	backgroundColor: Color.gray(20),
  valueColor: AlwaysStoppedAnimation(Color.blue),
  value: .5,
);

// CircularProgressIndicator
CircularProgressIndicator(
	backgroundColor: Color.gray[200],
  valueColor: AlwaysStoppedAnimation(Color.blue),
);

CircularProgressIndicator(
	backgroundColor: Colors.grey[200],
  valueColor: AlwaysStoppedAnimation(Color.blue),
  value: .5,
);

// 自定义尺寸





















```

































### 变量声明

var/dynamic/const/final

var

runtimeType: 用户获取变量的当前的类型

赋值之后，变量的类型不能修改

dynamic

赋值之后变量的类型可以修改，在程序中一般不使用

final & const

都是用于定义常亮，定义之后的值都不可以修改

区别？

const 在赋值时，赋值的内容必须是在编译期间就确定下来的

final 在赋值时，可以动态获取，比如赋值一个函数

const 时不可以赋值为 DateTime.now()

final 一旦被赋值后就有确定的后果，不会再次赋值

const放在赋值语句右边，可以共享对象，提高性能

### 数据类型

#### 数字类型

int double

#### 布尔类型

true false

#### 字符串

string

'1'/"2"/'''3'''/"""4"""

表达式的拼接

'${name}, ${age}, ${height}'

#### 集合类型

三种：List / Set / Map

List定义：

```js
// 类型推导
var letter = ['a', 'b', 'c'];
// 明确指定类型
List<int> numbers = [1, 2, 3, 4];
```

Set定义：

其实，就是把 [] 换成 {}。

Set 和 List 最大的两个不同就是，Set 是无序的，并且元素是不重复的。

```js
// 类型推导
var letter = {'a', 'b', 'c'};
// 明确指定类型
List<int> numbers = {1, 2, 3, 4};
```

Map 常说的字典类型

```js
// 类型推导
var infoMap = {"name": "who", "age": 12};
// 明确指定类型
Map<String, Object> infoMap2 = {"height": 1.99, "address": "beijing"};
```

常见的集合操作

```js
// 所有集合都支持获取长度
.length
```

添加/删除/包含操作

```js
// 对于List来说，元素是有序的，提供了一个删除指定索引位置上元素的方法
add
remove
contains
removeAt
```

Map操作

```js
// 1. 根据key获取valus
infoMap['name']
// 2.获取所有的entries
infoMap.entries
// 3.获取所有key
infoMap.keys
// 4.获取所有values
infoMap.values
// 5.判断是否包含某个key 或者 value
infoMap.containsKey('age')  infoMap.containsValue(18)
// 6. 根据key删除元素
infoMap.remove('age')
```

函数的定义

```js
返回值 函数名(参数列表) {
  函数体
  return 返回值
}
```



#### 运算符

```js
// 除法 /
// 整除 ～/
// 取模 %
// 赋值操作
var name = 'kkk';
var name2 = null;
name2 ??= 'xxx';
// 条件运算符
var temp = 'xxx';
var temp2 = null;
var name = temp ?? 'aaa';
// 当temp是null,返回aaa;
// 当temp不是null,返回temp的值。

// 级联语法
// 希望对一个对象进行连续的操作，这个时候可以使用级联语法

class Person {
  String name;

  void run() {
    print("${name} is running");
  }

  void eat() {
    print("${name} is eating");
  }

  void swim() {
    print("${name} is swimming");
  }
}

main(List<String> args) {
  final p1 = Person();
  p1.name = 'why';
  p1.run();
  p1.eat();
  p1.swim();

  final p2 = Person()
              ..name = "why"
              ..run()
              ..eat()
              ..swim();
}
```



### if和else

注意点：不支持非空即真或者非0即真，必须有明确的bool类型

### 循环操作

```js
for(var i = 0; i < 5; i++) {
  print(i)
}

// for in遍历List和Set
var names = ['a', 'b', 'c'];
for (var name in names) {
  print(name);
}
```

### 类和对象

```js
// 定义类使用 class 关键字
// 类通常有两部分组成，成员（menber）和方法（method）。
class 类名 {
  类型 成员;
  返回值类型 方法名(参数列表) {
    方法体
  }
}

使用类创建对象
main(List<String> args) {
  var p = new Person();
  p.name = 'xxx';
  p.eat();
}

注意：
在方法中使用属性（成员/实例变量）时，并没有加this;
会省略this，但是有命名冲突时，this不能省略；

// 使用类，创建对应的对象：
Dart2开始，new 关键字可以省略；
main(List<String> args) {
  // 1. 创建类的对象
  var p = new Person();
  // 2. 给对象的属性赋值
  p.name = 'xxx';
  // 3. 调用对象的方法
  p.eat();
}




```

#### 构造方法

##### 普通构造方法

当通过类创建一个对象时，会调用这个类的构造方法。

当类中没有明确指定的构造方法时。将默认拥有一个无参的构造方法。

前面的Person中就是在调用这个构造方法。

也可以根据自己的需求，定义自己的构造方法：

注意一：当有了自己的构造方法时，默认的构造方法将会失效，不能使用。

你可能希望明确的写一个默认的构造方法，但是会和我们自定义的构造方法冲突

Dart本身不支持函数的重载

注意二：这里还实现了toString方法

```js
class Person {
  String name;
  int age;
  Person(String name, int age) {
    this.name = name;
    this.age = age;
  }
  
  @override
  String toString() {
    return 'name=$name age=$age';
  }
}
```

另外，在实现构造方法时，通常做的事情就是通过参数给属性赋值

为了简化这一过程，Dart提供了一种更加简洁的语法糖形式。

上面的构造方法可以优化成下买的写法：

```js
Person(String name, int age) {
  this.name = name;
  this.age = age;
}
// 等同于
Person(this.name, this.age);
```

##### 命名构造方法

在开发中，我们确实想实现更多的构造方法，怎么办？

因为不支持方法（函数）的重载，所以我们没有办法创建相同名称的构造方法。

我们需要使用命名构造方法：

```js
class Person {
  String name;
  int age;
  
  Person() {
    name = '';
    age = 0;
  }
  
  // 命名构造方法
  Person.widthArgments(String name, int age) {
    this.name = name;
    this.age = age;
  }

	@override
	String toString() {
    return 'name=$name age=@age';
  }
}

// 创建对象
var p1 = name Person();
print(p1);
var p2 = new Personl.withArgments('xxx', 18);
print(p2);
```

在之后的开发中，我们也可以利用命名构造方法，提供更加便捷的创建对象的方法：

比如开发中，需要将一个Map转成对象，可以提供如下的构造方法

```js
// 新的构造方法
Person.fromMap(Map<String, Object>map) {
  this.name = map['name'];
  this.age = map['age'];
}
// 通过上面的构造方法创建对象
var p3 = new Person.fromMap({'name': 'xxx', 'age': 20});
print(p3);
```

##### 初始化列表

重新定义一个类Point, 传入x/y，可以得到到他们之间的距离distance：

```js
class Point {
  final num x;
  final num y;
  final num distance;
  
  Point(this.x, this.y): distance = sqrt(x*x + y*y);
}
// 上面这种初始化变量的方法，称之为 初始化列表（Initializer list）
```



##### 重定向构造方法

```js

```



### 事件循环

单线程模型中主要就是在维护着一个事件循环（Event Loop）。

事件循环是什么？

事实上事件循环并不复杂，它就是将需要处理的一系列事件（包括点击事件/IO事件/网络事件）放在一个事件队列（Event Queue）中。

不断的从事件队列中取出事件，并执行其对应需要执行的代码块，直到事件队列清空位置。

当我们有一些事件时，比如点击事件、IO事件、网络事件时，它们就会被加入到`eventLoop`中，当发现事件队列不为空时发现，就会取出事件，并且执行。

- 齿轮就是我们的事件循环，它会从队列中一次取出事件来执行。







































