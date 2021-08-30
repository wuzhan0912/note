---

---

## iframe 



 b端

前端定时调用过期么



马馨 陈柏宇



关于数据表细则 ，待会产品确定

先定义接口



1审核通过和配租状态的连续性

2 历史记录表的第一条数据!



CREATE TABLE t_public_rental_leasee (
  id bigint(20) NOT NULL,
  apply_no varchar(45) NOT NULL COMMENT '申请ID',
  city varchar(45) NOT NULL COMMENT '城市',
  area varchar(45) NOT NULL COMMENT '区域-市',
  block varchar(45) NOT NULL COMMENT '板块-乡',
  public_type tinyint(4) unsigned NOT NULL DEFAULT '0' COMMENT '公租房类型：1：公租房，2：人才房

3: 廉租房',
  `user_id` bigint(20) NOT NULL COMMENT '用户ID',
  `name` varchar(45) NOT NULL COMMENT '姓名',

  `identity` varchar(32) NOT NULL COMMENT '身份证号码',

  `sex` int(11) NOT NULL COMMENT '性别:1:男,2:女',
  `mobile` varchar(45) NOT NULL COMMENT '联系人电话',
  `work_status` tinyint(4) NOT NULL COMMENT '工作状态 1:企业，2:个体公商户，3:灵活商户，4:退休，5:机关事业单位',
  `work_unit` varchar(50) NOT NULL COMMENT '工作单位',
  `work_address` varchar(100) NOT NULL COMMENT '工作地址',
  `marriage` tinyint(4) NOT NULL COMMENT '婚姻状况 1:已婚，2:未婚，3:离异，4:寡鳏',
  `register_address` varchar(100) DEFAULT NULL COMMENT '户口所在地',
  `domiciliary_register` tinyint(4) NOT NULL COMMENT '户籍 1:农业，2:非农业',
  `address` varchar(100) NOT NULL COMMENT '住址',
  `residence_time` varchar(20) NOT NULL COMMENT '本市居住时间',
  `low_no` varchar(45) NOT NULL COMMENT '低保证号',
  `pension` tinyint(4) NOT NULL COMMENT '是否缴纳养老保险',
  `medicare` tinyint(4) NOT NULL COMMENT '是否缴纳医疗保险',
  `accumulation_fund` tinyint(4) NOT NULL COMMENT '是否缴纳公积金',
  `provide_type` tinyint(2) unsigned NOT NULL COMMENT '发放方式  1 租赁补  2  实物配租',
  `pension_insurance_date` varchar(45) DEFAULT NULL COMMENT '养老保险缴纳时间',
  `insurance_date` varchar(45) NOT NULL COMMENT '医疗保险缴纳时间',
  `accumulation_date` varchar(45) NOT NULL COMMENT '公积金缴纳时间',
  `monthly_income` int(11) NOT NULL COMMENT '个人月收入',
  `family_monthly_income` int(11) NOT NULL COMMENT '家庭月收入',
  `member_family` text NOT NULL COMMENT '家庭成员，json',
  `declare_family_num` int(11) NOT NULL COMMENT '申报家庭人数总数',
  `real_family_num` int(11) NOT NULL COMMENT '实报家庭人数总数',
  `family_member_type` varchar(45) NOT NULL COMMENT '家庭成员类型 1:景洪市户籍城镇居民，2:农转城居民，3:大中专院校及职校毕业生，4:引进人才，5:全国,省部级劳模，6:全国劳模，7:荣立二等功以上的复转军人，8:景洪市进城务工人员，9:景洪市外来务工人员，10:烈暑，11:重度残疾，12:重病，孤寡，年老，13:其他',
  `house_address` varchar(45) NOT NULL COMMENT '房产地址',
  `house_area` int(11) NOT NULL COMMENT '房产面积',
  `property_rights` int(11) NOT NULL COMMENT '产权性质 1:房改房，2:经济适用房（全额集资房），3:商品房，4:自由私有（含农村住宅），5:承租公房，6:临时简易房，7:暂住亲友家,8:其他',
  `other` text NOT NULL COMMENT '其他信息,json',
  `deposit_amount` int(11) unsigned NOT NULL COMMENT '存款',
  `real_estate_amount` int(11) NOT NULL COMMENT '房产',
  `car_amount` int(11) NOT NULL COMMENT '车辆价值',
  `security_amount` int(11) NOT NULL COMMENT '证卷价值',
  `other_property` int(11) NOT NULL COMMENT '其他财产',
  `accept_no` varchar(100) NOT NULL COMMENT '线下受理编号',
  `application_number` varchar(45) NOT NULL COMMENT '申请书编号',
  `publicity_date` varchar(45) NOT NULL COMMENT '公示日期',
  `publicity_remarks` varchar(45) NOT NULL COMMENT '公示备注',
  `remarks` varchar(500) NOT NULL COMMENT '备注',
  `config_rent_money` int(11) unsigned NOT NULL COMMENT '配租租金 单位：分 ',
  `allot_project` varchar(50) NOT NULL COMMENT '分配到项目',
  `allot_community` varchar(45) NOT NULL COMMENT '分配到小区',
  `allot_building` varchar(45) NOT NULL COMMENT '分配到楼栋',
  `allot_house` varchar(45) NOT NULL COMMENT '分配到房间',
  `apply_status` int(11) NOT NULL COMMENT '申请状态',
  `reason` varchar(45) NOT NULL COMMENT '原因说明',
  `source` int(11) NOT NULL DEFAULT '1' COMMENT '订单来源：1线上，2线下',
  `feedback` text NOT NULL COMMENT '回馈信息，json',
  `operator_id` int(11) unsigned NOT NULL DEFAULT '0' COMMENT '操作人id',
  `operator_name` varchar(50) NOT NULL DEFAULT '' COMMENT '操作人姓名',

check_in_status int (11) NOT NULL DEFAULT '0' COMMENT '入住状态：0未入住，1已入住',

`deleted` tinyint(4) NOT NULL DEFAULT '0' COMMENT '删除状态:0：有效，1:删除',
`create_by` bigint(20) NOT NULL DEFAULT '0' COMMENT '创建人',
`update_by` bigint(20) NOT NULL COMMENT '更新人',
`create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '添加时间',
`update_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',

  PRIMARY KEY (`id`),
  KEY `index1` (`apply_no`)
) ENGINE=InnoDB  DEFAULT CHARSET=utf8 COMMENT='公租房承租人表'



`&#8212;` is the decimal-encoded equivalent of `&mdash;`.

Use the `printf()` function.

/data1/www/java_release/tenant/zulin-manage/bin

t_public_rental_lease_snap

morkuser 和 jpa赋值

vim 翻页 ctrl b ctrl f

首页入口有一个字典项