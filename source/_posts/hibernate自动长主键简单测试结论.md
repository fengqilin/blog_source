---
title: 关于hibernate自增长键的解释
tags: [hibernate]
categories: [hibernate,收集]
---

工作之初，遇到一个问题是，在集群中，多个hibnate对一个数据库操作的主键重复问题
大家都认为采有自增长会产生重复主键。事实情况是怎么样的呢？
让我们真实情部下了解一下。
测试环境：spring+hibernate5,数据库主键字段设置为自增长。
打开hibernate的sql输出，开始mysql的binlog日志。
比如我的测试环境中，

	@Id
	@GenericGenerator(name = "generatorId", strategy = "native")
	// @GeneratedValue(generator = "generatorId") //和生成策略名一样 或者
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	// 根据底层数据库自动选择方式，需要底层数据库的设置 如MySQL，会使用自增字段，需要将主键设置成auto_increment。
	@Column(name = "id")
	@Type(type = "int")
	private int id;

	@Column(name = "name", length = 200)
	private String name;

	@Column(name = "password", length = 200 )
	private String passowrd;

	@Column(name = "birthday", columnDefinition = "timestamp")
	private Date birthday;

	省略getter/setter等

在这样的配置下，会观察到输出为：
	DEBUG SQL:92 - insert into user (birthday, name, password) values (?, ?, ?)

hibernate根本就没有设置id，id是数据库自动生成的，数据库会保证id唯一。
我们再观查 binlog日志，
	SET TIMESTAMP=1459431449/*!*/;
	SET @@session.time_zone='SYSTEM'/*!*/;
	insert into user (birthday, name, password) values ('2016-03-31 21:37:29', 'feng', null)	

所以id也正如解释的那样，和hibernate一点关系也没有，集群操作中，这个id都是由数据库来保证唯一的，
hibernate不是很多人认为的那样，先一个获取了一个count（）的值来维持 自增长。		

