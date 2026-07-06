如下为目前PDD category在进行Gtin 简称/Outer简称时会用到的Gtin level字段：

Consumer Unit Barcode
CN Category
Item Chinese Description
DP Level3 En
DP Level3 Cn
Product_Line_En
CN Variant
CN Full Variant Chinese
CN Variant Chinese
CN Sub Variant Chinese
Shave Sub Family
CN Size Combined

By Category 配置的规则为：

 	GTIN_code	GTIN_short	GTIN size	GTIN line up	GTIN subproduct	GTIN 中文	GTIN 功效	参考简称
HC	Consumer Unit Barcode	公式后六位	CN Size Combined	DP Level3 En	CN Variant	CN Full Variant Chinese	CN Variant Chinese	GTIN 功效&GTIN size
PCC	Consumer Unit Barcode	公式后六位	CN Size Combined	Product_Line_En	DP Level3 En	DP Level3 Cn	CN Sub Variant Chinese	GTIN 功效&GTIN size
OCNP	Consumer Unit Barcode	公式后六位	CN Size Combined	DP Level3 En	CN Variant	CN Full Variant Chinese	CN Sub Variant Chinese	GTIN 功效&GTIN size
Shave	Consumer Unit Barcode	公式后六位	CN Size Combined	DP Level3 En	CN Variant	CN Variant Chinese	Shave Sub Family	GTIN 功效&GTIN size
SC	Consumer Unit Barcode	公式后六位	CN Size Combined	Product_Line_En	DP Level3 En	CN Full Variant Chinese	CN Variant Chinese	GTIN 功效&GTIN size
FEM	Consumer Unit Barcode	公式后六位	CN Size Combined	Product_Line_En	DP Level3 Cn	CN Variant Chinese	CN Variant Chinese	GTIN 功效&GTIN size
BC	Consumer Unit Barcode	公式后六位	CN Size Combined	CN Variant	DP Level3 En	DP Level3 Cn	DP Level3 Cn	GTIN 功效&GTIN size
FHC	Consumer Unit Barcode	公式后六位	CN Size Combined	DP Level3 Cn	Product_Line_En	CN Full Variant Chinese	CN Variant Chinese	GTIN 功效&GTIN size
Power	Consumer Unit Barcode	公式后六位	CN Size Combined	DP Level3 Cn	Product_Line_En	CN Variant Chinese	CN Sub Variant Chinese	GTIN 功效&GTIN size

基于Gtin简称生成outer ID属性的逻辑为： @Du, Jianyao Candice确认一下一下outer ID line up/sub product的判断逻辑OK不OK

字段	判断逻辑	备注
Outer ID		
Line up	
如果一个 Outer ID 下包含了多个不同的Gtin Line up，则以其中Gtin Size最大的Line up来定义这个 Outer ID 下所有SKU的 Lineup	需要增加自定义功能，同时需要有去除空格等规范化操作
Sub Product	Gtin Sub Product，如果一个 Outer ID 下包含了多个不同的Gtin Sub Product，则以其中Gtin Size最大的Line up来定义这个 Outer ID 下所有SKU的 Sub Product	需要增加自定义功能，同时需要有去除空格等规范化操作
Short Name	假如outer ID为单品，为GTIN简称
假如Outer ID为组品，为GTIN简称*支数+GTIN简称*支数+...	以GP生成outer ID最终规则为准
需求为需要以GTIN简称组成最终outer ID short name
Differentiation_tag	人工	

