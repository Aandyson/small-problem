# 正则表达式 #

#### 动机 ####
1. 处理文本已经成为计算机主要的工作内容
2. 根据文本的内容进行固定的筛选匹配是文本处理中常用的工作之一
3. 为了快速方便的解决上述问题，正则表达式应运而生，并且逐渐发展为单独的计算。可以被众多的编程语言使用

定义:

- 高级的文本模式匹配，提供了搜索和替代等功能，本质是由一些字符和特殊符号组成的字符串。这个字符串描述了这些字符和字符的某种重复方式，按照这种方式可以匹配一个有相似特征的符合规则的字符集合

目标：

1. 理解什么是正则表达式及正则表达式的用途
2. 能够掌握正则表达式的规则和用法
3. 能够使用python re模块进行基于正则表达式的文本操作 

特点和用途

- 使用编程进行文本的检索和修改
- 正则表达式支持语言众多，使用方便
- 正则表达式可以匹配一系列字符串而不是某一个，匹配更多样化
- 在mongo中可以存储正则表达式，也可以使用正则表达式做查找操作
- 大量使用在爬虫中国用来处理HTML文本匹配


## 正则表达式规则和用法 ##

#### 元字符的使用 ####

re.findall(regex,string)

	功能：在string字符串中，匹配正则表达式能够匹配的项，放到一个列表中返回

- 普通字符串
	
	元字符： abc 

	匹配规则：匹配字符串的值

	匹配示例：abc

- 使用‘或’进行多个匹配
	
	元字符:	re1 | re2  ('|'表示或)

	匹配规则: 既能匹配正则表达式re1所表达内容，也能匹配re2所表达内容

	匹配示例： ab | bc --> ab bc

	eg：

	    In : re.findall('ab|ef','abcdef')
    
    	Out: ['ab', 'ef']

- 点号 "."

	元字符： . 

	匹配规则：匹配任意一个字符

	匹配示例：f.o --> foo fao f@o 等等
		    相当于通配符中的"?"
	eg：

	    In : re.findall('f.','a1fsdgsda1df,gfdf@')
    
    	Out: ['fs', 'f,', 'fd', 'f@']
    
- 匹配开头字符串

	元字符: ^

	匹配规则：匹配一个字符串的开头位置

	匹配示例：^from 匹配以from开的头的字符串启示部分

	eg：

	    In : re.findall('^from','from china')
    
    	Out: ['from']
    
    	In : re.findall('^from','I come from china')
    
    	Out: []

- 匹配结尾字符串

	元字符： $

	匹配规则： 当一个字符串以什么结尾时使用$标记

	
	匹配示例： py$  匹配所有以py结尾的字符串
	
	eg：
	
    	In : re.findall('py$','test.py')
    	Out: ['py']

- 匹配任意零个或多个字符

	元字符： * 

	匹配规则： 匹配*前面出现的字符0次或多次

	匹配示例：ab* --> abbbbbbbb

	eg:
	
	    `In : re.findall('ab*','abbb,ab')
		Out: ['abbb', 'ab']
	     
		# '.*' 匹配任意字符串                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            
		In : re.findall('.*','abbb,abggg')
		Out: ['abbb,abggg', '']
		
		In : re.findall('.*','abbb,abggg')
		Out: ['abbb,abggg', '']
		
		In : re.findall('ab*','abbb,abgg,a')
		Out: ['abbb', 'ab','a']
		`
- 匹配任意1个或多个字符


	元字符： +

	匹配规则： 匹配+号前面出现的字符或正则表达式一次或者多次

	匹配示例：  ab -->  abbbbbbbbbb

	eg:
		
		In : re.findall('ab+','abbb,abgg,a')
		Out: ['abbb', 'ab']

> ".*"可以匹配空字符串，",+"不能匹配空字符串


- 匹配字符串0次或1次

	元字符： ？

	匹配规则： 匹配前面出现的字符或正则表达式0次或1次

	匹配示例：ab？ --> a 或 ad

	eg:

		In [45]: re.findall('ab?',"abbb,a,abb")
		Out[45]: ['ab', 'a', 'ab']

- 匹配前面的字符或re指定次数

	元字符： {N}

	匹配规则：匹配前面出现的字符或正则表达式N次

	匹配示例：ab{3} --> abbb

	eg:

        In : re.findall('ab{2}',"abbbb,ab,abbcc")
    	Out: ['abb', 'abb']

- 匹配前面的字符或re指定次数	
	
	元字符：{M,N}   M，N代表数字

	匹配规则：匹配前面出现的字符或正则表达式M次 **到** N次

	匹配示例： ab{3，8} --> abbb, ... ,abbbbbbbb

	eg:

	    In : re.findall('ab{2,4}',"abbbbbbbbb,ab,abbcc")
	    Out: ['abbbb', 'abb']
	    
- 字符集合匹配

	元字符： [abcd]

	匹配规则：匹配中括号的任意**一个**字符(只能一个字符)

	匹配示例：b[abcd]t --> bat bbt  bct bdt

	eg:
	
    	In : re.findall('b[abcd123]t',"bat,b1t,b111")
    	Out: ['bat', 'b1t']


- 字符集合匹配

	元字符： [a-zA-Z0-9]

	匹配规则：匹配中括号的任意一个**区间**内的字符

	匹配示例：[a-zA-Z0-9]+ --> 匹配任意一个由字母数字组成的非空字符串(只能一个)

	eg：

		只能匹配范围内的一个
		In : re.findall('[a-zA-Z]',"bat,b1t,b111sdfsd")
		Out: ['b', 'a', 't', 'b', 't', 'b', 's', 'd', 'f', 's', 'd']
		只能匹配范围内的		
		In : re.findall('[a-zA-Z]+',"bat,b1t,b111sdfsd")
		Out: ['bat', 'b', 't', 'b', 'sdfsd']
  	
    	In : re.findall('[a-zA-Z0-9]+',"bat,b1t,b111sdfsd")
    	Out: ['bat', 'b1t', 'b111sdfsd']
		
		In : re.findall('[a-zA-Z0-9]+',"bat,b@@1t,b111#$%s^&*dfsd")
		Out: ['bat', 'b', '1t', 'b111', 's', 'dfsd']

  
- 字符集合不匹配

	元字符： [^...]   ...表示上面上两项中的任意内容

	匹配规则：匹配的任意非中括号中的字符集

	匹配示例：[^asd]+ --> 匹配任意**一个**非asde字符,
			 [^a-z]+ --> 匹配任意**一个**非a-z字符

	eg:
		
		In : re.findall('[^a-z]',"bat,b@@1t,b1#$%s^,AF")
		Out: [',', '@', '@', '1', ',', '1', '#', '$', '%', '^', ',', 'A', 'F']


- 匹配(非)数字字符

	元字符： \d[0-9]  \D[^0-9]

	匹配规则：\d 匹配任意**一个**数字字符，\D 匹配任意**一个**非数字字符

	匹配示例：\d{3} --> 匹配任意一个单位数

	eg:

		In : re.findall('\d{3}',"31243ss 25fsd345")
		Out: ['312', '345']
		
		In : re.findall('\D{3}',"31243ss 25fsd345")
		Out: ['ss ', 'fsd']
> 空格也算是字符



- 匹配(非)字母数字字符

	元字符： \w [a-zA-Z0-9]  \W [^a-zA-Z0-9]

	匹配规则：\w 匹配任意**一个**字母或数字字符，\W 匹配任意**一个**非字母或数字字符

	匹配示例：\w{3} --> 任意三个数字或字母

	eg

		In : re.findall('[A-Z]\w*',"Anzhen Hello Word")
		Out: ['Anzhen', 'Hello', 'Word']

		In : re.findall('\w+-\d+',"Anzhen-33 Hello Word")
		Out: ['Anzhen-33']

		In : re.findall('\w+-\W+',"Anzhen-33 Hello Word-^%$#")
		Out: ['Word-^%$#']

- 匹配(非)空字符

	元字符： \s (空格 \n \0 \t \r(回车))   \S 

	匹配规则：\s 匹配任意**空**字符，\S 匹配任意**非空**字符

	匹配示例：hello\sword  --> hello word

	eg:

		In : re.findall('hello\s+word','hello  word')
		Out: ['hello  word']

		In : re.findall('\s','hello@#$ word\n')
		Out: [' ', '\n']

		In : re.findall('\S*','hello@#$ word')
		Out: ['hello@#$', '', 'word', '']

- 匹配字符串开头和结尾


	元字符： \A(^)  \Z($) 

	匹配规则：\A 匹配字符串的开头位置，\Z 匹配字符串的结尾位置

	匹配示例：\Aabc\z  == ^abc$  严格的匹配abc

	eg:

		In : re.findall('\Aa','abaaacvv')
		Out: ['a']
		In : re.findall('v\Z','abaaacv,v')
		Out: ['v']
		In : re.findall('\Aabc\Z','abcabc')
		Out: []

- 匹配(非)单词边界

	元字符： \b  \B

	匹配规则：将非字母的部分不认为是单词部分，将连续字母的部分认为是一个单词

	匹配示例：

	eg:

    	In : re.findall('\btest\b','This is a %test%')
    	Out: []  # 不加 r 匹配不到

    	In : re.findall(r'\btest\b','This is a %test%')
    	Out: ['test']
    	
    	In : re.findall(r'\bis\b','This is a %test%')
    	Out: ['is']  # 单词is
    	
    	In : re.findall(r'\Bis\b','This is a %test%')
    	Out: ['is']  # this 中的 is 
    
## 元字符总结 ##

	字符 ： 匹配实际字符       
	匹配单个字符：  .   []   \d  \D  \w \W  \s  \S
	匹配重复次数： *   +   ？  {} 
	匹配开头结尾： ^  $  \A  \Z   \b  \B
	其他 ： |    [^ ]


## raw字串和转义 ##

- r'This is a %test%' --> 转化为raw字串

- raw字串**特点**: 不进行转义解析

- ‘'This is \n a %test%'’(换行) --> **r**'This is \n a %test%' (不换行)

什么时候加**r**

- 转变为raw字符串是为了防止python对字符串的转义解析，所以在正则表达式本身有‘\’的时候最好加上r

- 正则表达式的转义匹配
- 当匹配正则表达式内的特殊字符的时候，正则表达式本身也需要进行转义，如果要匹配的字符串中的*，则正则表达式应为“\”

 \  *  ?  .  ()  []  {}

		匹配*
	    In : re.findall(r'\*','This is *a %test%')
	    Out: ['*']

		匹配 \
		In : re.findall(r'\\','This is\ a %test%')
		Out: ['\\']  # 一个'\'
		
		In : re.findall(r'\\','This is\\ a %test%')
		Out: ['\\']  # 一个'\'
		
		In : re.findall(r'\\','This is\\\ a %test%')
		Out: ['\\', '\\']  # 两个'\'
		
		In : re.findall(r'\\','This is\\\\ a %test%')
		Out: ['\\', '\\']
		
		In : re.findall(r'\\','This is\\\\\ a %test%')
		Out: ['\\', '\\', '\\']



## 贪婪和非贪婪 ##

#### 贪婪模式 ####

- 不做处理的情况下，正则表达式默认是贪婪模式，即在使用 * + ? {} 的时候尽可能多的向后进行匹配

    	eg:
		# ab* 可以匹配 a ab abb... 那么当b足够多的时候它会尽可能多的去匹配
    	In : re.findall(r'ab*','abbbb')
    	Out: ['abbbb']
  		# 其他情况同理
    	In [114]: re.findall(r'ab+','abbbb')
    	Out[114]: ['abbbb']
    	
    	In [115]: re.findall(r'ab?','abbbb')
    	Out[115]: ['ab']
    	
    	In [116]: re.findall(r'ab{2,4}','abbbb')
    	Out[116]: ['abbbb']


#### 非贪婪模式 ####

-尽可能少的匹配符合正则条件的内容

- 贪婪模式 --> 非贪婪模式的方法 --> 后面加？ 
- 即 "*？"   "+？"  "？？"  "{M,N}？"

    	eg:

    	In : re.findall(r'ab*?','abbbb')
    	Out: ['a']
    	
    	In : re.findall(r'ab+?','abbbb')
    	Out: ['ab']
    	
    	In : re.findall(r'ab??','abbbb')
    	Out: ['a']
    	
    	In : re.findall(r'ab{2,4}?','abbbb')
    	Out: ['abb']


## 正则表达式分组 ##

-  正则表达式可以分组，分组的标志为括号()，每个括号都是正则表达式的一个子组，而每个子组是整体正则表达式的一部分，同时也是一个小的正则表达式

    	In : re.findall(r'((ab)*(cd))','abababcd')
    	Out: [('abababcd', 'ab', 'cd')]
> 子组只匹配一次     

-  当有多个子组的时候，我们从外层向内层分别叫第一，第二 ... 子组。当同一层次的时候，从左向右分别计数

- 分组会改变 * + ？ {} 的重复行为，即吧每个分组当做一个整体对待，进行相应的重复操作 


-  当子组能够和多个目标字符串内容进行匹配时，findall只返回子组的内容，且值返回一个，不返回整体的内容，要想返回整体需要再加括号

		In : re.findall(r'((ab)+)cd','abababcdsdf')
    	Out: [('ababab', 'ab')]
    	
    	In : re.findall(r'(((ab)+)cd)','abababcdsdf')
    	Out: [('abababcd', 'ababab', 'ab')]

- 每个组都可起名字，我们可以根据起的名字辨别各个组 

	- 名字格式: (?P<word>hello
	- 给子组(hello)起个名字，这个名字是"word"
	- 子组通过名字进行调用(?P=word) 表示入职子组正则表达式内容

	    In [138]: re.findall(r'((?P<aaa>hello)\s+(?P=aaa))','hello hello')

	    Out[138]: [('hello hello', 'hello')]

----------------------------------
----------------------------------
练习：
  
1. 匹配长度8-10位的密码，必须字母开头，数字字母下下线组成     
  
    ^[a-zA-Z][_a-zA-Z0-9]{7,9}$

2. 匹配简单的身份证号

    \d{17}(\d | X)  


# re #

说明：

1. 以下七个函数是正则表达式匹配的常用函数
2. 这起个函数即可以使用re直接调用，也可以使用re.compile 生成的正则表达式对象来调用，其功能完全相同
3. 使用re模块直接调用的时候这些函数的第一个参数需要传入正则表达式。使用re.compile 对象调用的时候，因为已经确定了正则表达式，所以不需要再传入

		eg: 
		re.findall(r'\w+','hello word')
		
		obj = re.compile(r'\w+')
		obj.findall('hello word')

#### findall ####
- 获取字符串中所有能被正则表达式匹配的字串，以列表返回，如果加子组（括号），则返回每个子组中的内容


	    `import re
	
		pattern = r'\w+'
	
		# 获取正则表达式对象
		re_obj = re.compile(pattern)
		
		# print(dir(re_obj))
	
		L = re_obj.findall('hello word')
		print(L)
		`

#### finditer ####
- 同findall，只是返回的是一个迭代器，每次迭代取得的是一个match对象

#### split ####

- 将一个字符串，按照正则表达式的匹配内容进行分割，得到分割后的列表


#### sub ####

- sub(restring,restring,max=0)
- 将正则表达式匹配的string字符串的内容替换为restring
- max：如果不写则表示全部替换，如果赋值则表示最多替换max出

#### subn ####

- 同sub只不过返回值多一个实际替换的个数

示例/reget_test.py


    import re
    
    pattern = r'\w+'
    
    # 获取正则表达式对象
    re_obj = re.compile(pattern)
    # print(dir(re_obj))
    
    L = re_obj.findall('hello word')
    # print(L)
    
    # 生成查找结果的迭代器
    # group可以取得match的值
    for i in re_obj.finditer('hello word'):
    	print(i.group())
    
    # 按照正则表达式匹配的内容进行分隔
    L1 = re.split(r'\s', 'hello word　sdfd name zxd')
    print(L1)
    
    # 将正则匹配到的部分替换为"****",最多替换3处
    s = re.sub(r'[A-Z]', '****', 'Hello Word, I Love China', count=3)
    print(s)
    
    # 比sub多一个数-->实际替换的个数
    s1 = re.subn(r'[A-Z]', '****', 'Hello Word, I Love China', count=3)
    print(s1)



#### match ####

- 匹配正则表达式对应的内容，但是只能匹配**一项**
- match所匹配的内容要求**必须**在正则表达式的启示位置否则匹配不到
- match匹配到对应内容返回match object 否则返回None

#### search ####

-同match

- 匹配正则表达式对应的内容，但是只能匹配**一项**
- search匹配到对应内容返回match object， 否则返回None
- search所匹配的内容并**不一定**在正则表达式的启示位置


#### 获取match对象 ####

1. finditer 得到迭代对象，迭代的每一项都是一个match object
2. match函数匹配的结果返回一个match object
3. search函数匹配到的结果返回一个match object

示例/reget_test1.py


    import re
    
    re_obj = re.compile(r'foo')
    
    # 匹配字符串中符正则规则的项
    # foo　不在开头的位置否则匹配不到，报错
    # 有多个只能匹配一个
    match_obj = re_obj.match('food on the table foo')
    
    print(match_obj)
    
    print(match_obj.group())
    
    print('-----------------------------')
    
    # 匹配字符串中符正则规则的项
    # foo 并不一定在开头
    # 有多个只能匹配一个
    match_obj = re_obj.search('This is my food　food')
    
    print(match_obj)
    
    print(match_obj.group())


-作用:

	通过match object的属性和函数可以获取更加详细的匹配信息

match常用的属性
	
	re  pos  endpos  lastgroup  lastindex

match常用的方法

	start()  end()  span()  group()  groups()   groupdict()


示例/reget_test2.py


	import re
	
	re_obj = re.compile('(ab)cd(?P<name>ef)(?P<ZZZZ>gh)')
	
	match_obj = re_obj.search("abcdefghi")
	
	# 获取对应的郑则表式对象
	print(match_obj.re)
	# 匹配到的目标字符串的起始位置
	print(match_obj.pos)
	# 匹配到的目标字符串的结束位置后一个
	print(match_obj.endpos)
	# 最后一组的组名
	print(match_obj.lastgroup)
	# 共有几组
	print(match_obj.lastindex)
	
	print('==========以下为方法=====================')
	
	re_ob = re.compile('(ab)cd(?P<name>ef)')
	
	match_ob = re_ob.search("hi,abcdefghi")
	
	# 得到匹配字符串在目标字符串中的起始位置
	print(match_ob.start())
	# 得到匹配字符串在目标字符串中的结束位置的下一个
	print(match_ob.end())
	# 得到匹配字符串在目标字符串中的起始和结束的位置
	print(match_ob.span())
	
	# 默认参数为0即获取整个正则表达式所获取到的内容
	# 若是个大于0的正整数则表示获取相应子组代表的内容
	# 过大于子组个数则报错
	print(match_ob.group())
	print(match_ob.group(1))
	print(match_ob.group(2))
	# print(match_ob.group(3))
	
	# 返回全部子组匹配内容，以一个元组形式打印
	print(match_ob.groups())
	
	# 获取所有命名组的名字和键值关系，组名为键，匹配内容为值
	print(match_ob.groupdict())




### flags ###

-re模块提供的正则表达式的标示位 flags

- I, IGNORECASE  忽略大小写

- S, DOTALL     可匹配换行 对元字符"."其作用

- M, MULTILINE  匹配开头结尾时计算换行，对^ $ 元字符有作用

- X, VERBOSE    给正则表达式加注释，但实际应用汇显得乱，不建议大量使用

如果同时使用多个标识符用竖线隔开

re.I | re.S
 
示例/reget_tes3.py

	import re
	
	# I忽略大小写
	re_obj = re.compile(r'ab', re.I)
	
	print(re_obj.search('ABcd').group())
	
	
	print('--------------S--------------')
	
	
	s = '''hello 
	 word'''
	# '.' 不能匹配换行符，但是加上re.S标示后即可匹配换行
	print(re.findall(r'.*', s))
	print(re.findall(r'.*', s, re.S))
	
	print('--------------M--------------')
	
	# 将字符串每行的开头结尾变成可被^和$匹配
	print(re.findall(r' word$', s, re.M))
	
	print('--------------X--------------')
	
	re_obj = re.compile(r'''
	    (ab)  # this group 1
	    cdef  # others
	    ''', re.X)
	
	print(re_obj.search('abcdefg').group())



# 结束 #

