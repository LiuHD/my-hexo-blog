---
title: 详解Mysql Explain 功能中的type字段含义
date: 2018-04-19 13:37:12
tags: ["mysql","explain","sql调优"]
---

## 准备工作

为了接下来方便演示 EXPLAIN 的使用, 首先我们需要建立两个测试用的表, 并添加相应的数据:
<!-- more -->
```sql
CREATE TABLE `user_info` (
  `id`   BIGINT(20)  NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(50) NOT NULL DEFAULT '',
  `age`  INT(11)              DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `name_index` (`name`)
)
  ENGINE = InnoDB
  DEFAULT CHARSET = utf8;

INSERT INTO user_info (name, age) VALUES ('xys', 20);
INSERT INTO user_info (name, age) VALUES ('a', 21);
INSERT INTO user_info (name, age) VALUES ('b', 23);
INSERT INTO user_info (name, age) VALUES ('c', 50);
INSERT INTO user_info (name, age) VALUES ('d', 15);
INSERT INTO user_info (name, age) VALUES ('e', 20);
INSERT INTO user_info (name, age) VALUES ('f', 21);
INSERT INTO user_info (name, age) VALUES ('g', 23);
INSERT INTO user_info (name, age) VALUES ('h', 50);
INSERT INTO user_info (name, age) VALUES ('i', 15);
CREATE TABLE `order_info` (
  `id`           BIGINT(20)  NOT NULL AUTO_INCREMENT,
  `user_id`      BIGINT(20)           DEFAULT NULL,
  `product_name` VARCHAR(50) NOT NULL DEFAULT '',
  `productor`    VARCHAR(30)          DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `user_product_detail_index` (`user_id`, `product_name`, `productor`)
)
  ENGINE = InnoDB
  DEFAULT CHARSET = utf8;

INSERT INTO order_info (user_id, product_name, productor) VALUES (1, 'p1', 'WHH');
INSERT INTO order_info (user_id, product_name, productor) VALUES (1, 'p2', 'WL');
INSERT INTO order_info (user_id, product_name, productor) VALUES (1, 'p1', 'DX');
INSERT INTO order_info (user_id, product_name, productor) VALUES (2, 'p1', 'WHH');
INSERT INTO order_info (user_id, product_name, productor) VALUES (2, 'p5', 'WL');
INSERT INTO order_info (user_id, product_name, productor) VALUES (3, 'p3', 'MA');
INSERT INTO order_info (user_id, product_name, productor) VALUES (4, 'p1', 'WHH');
INSERT INTO order_info (user_id, product_name, productor) VALUES (6, 'p1', 'WHH');
INSERT INTO order_info (user_id, product_name, productor) VALUES (9, 'p8', 'TE');

```

`type` 字段比较重要，它提供了判断查询是否高效的重要依据，通过它，我们可以判断此次查询是`全表扫描`还是`索引扫描`等。

## type常用类型

- `system` 表中只有一条数据。这个类型其实是特殊的const类型。
- `const` 针对主键或唯一索引的等值查询扫描，最多只返回一行数据, `const`查询速度非常快，因为它仅仅读取一次即可。
	例如，
	```
	EXPLAIN SELECT * FROM 
	student_score WHERE id = 1;

	+----+-------------+---------------+-------+---------------+---------+---------+-------+------+-------+
	| id | select_type | table         | type  | possible_keys | key     | key_len | ref   | rows | Extra |
	+----+-------------+---------------+-------+---------------+---------+---------+-------+------+-------+
	|  1 | SIMPLE      | student_score | const | PRIMARY       | PRIMARY | 4       | const |    1 |       |
	+----+-------------+---------------+-------+---------------+---------+---------+-------+------+-------+
	1 row in set

	```

	这条语句就是使用的const type类型的查询。
	而
	```SQL
	EXPLAIN SELECT * FROM student_score WHERE id = 1 OR id =2;

	+----+-------------+---------------+-------+---------------+---------+---------+------+------+-------------+
	| id | select_type | table         | type  | possible_keys | key     | key_len | ref  | rows | Extra       |
	+----+-------------+---------------+-------+---------------+---------+---------+------+------+-------------+
	|  1 | SIMPLE      | student_score | range | PRIMARY       | PRIMARY | 4       | NULL |    2 | Using where |
	+----+-------------+---------------+-------+---------------+---------+---------+------+------+-------------+
	1 row in set

	```
	这条语句就不是`const` 类型了, 而是`range`类型.

- `range` 表示使用索引范围查询,通过索引字段范围获取表中部分数据记录.这个类型通常出现在=, <>, >, >=, <, <=, IS NULL, <==>, BETWEEN, IN()操作中.
	当`type`是`range`时, 那么EXPLAIN输出的ref字段为null, 并且`key_len`字段是此次查询中使用到的索引的最长的那个.

	```
	EXPLAIN SELECT * FROM student_score WHERE id BETWEEN 2 AND 8;
	+----+-------------+---------------+-------+---------------+---------+---------+------+------+-------------+
	| id | select_type | table         | type  | possible_keys | key     | key_len | ref  | rows | Extra       |
	+----+-------------+---------------+-------+---------------+---------+---------+------+------+-------------+
	|  1 | SIMPLE      | student_score | range | PRIMARY       | PRIMARY | 4       | NULL |    6 | Using where |
	+----+-------------+---------------+-------+---------------+---------+---------+------+------+-------------+

	其中key_len取值是主键(int类型)的索引长度4
	```

- `eq_ref` 此类型通常出现在多表的join查询, 表示对前表的每一个结果, 都只能匹配到后表的一行结果, 并且查询的比较操作通常是`=`, 查询效率较高.

	```SQL
	EXPLAIN SELECT * FROM user_info, order_info WHERE user_info.id = order_info.user_id;
	+----+-------------+------------+--------+---------------------------+---------------------------+---------+----------------------------------+------+-------------+
	| id | select_type | table      | type   | possible_keys             | key                       | key_len | ref                              | rows | Extra       |
	+----+-------------+------------+--------+---------------------------+---------------------------+---------+----------------------------------+------+-------------+
	|  1 | SIMPLE      | order_info | index  | user_product_detail_index | user_product_detail_index | 254     | NULL                             |    9 | Using index |
	|  1 | SIMPLE      | user_info  | eq_ref | PRIMARY                   | PRIMARY                   | 8       | student_score.order_info.user_id |    1 |             |
	+----+-------------+------------+--------+---------------------------+---------------------------+---------+----------------------------------+------+-------------+
	```


