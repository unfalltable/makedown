## POI和JXL的问题

- 内存占用大，有内存溢出风险
- 学习成本高，实现复杂

## POI和EasyExcel的工作原理

![image-20221016145115854](C:\Users\BDA\AppData\Roaming\Typora\typora-user-images\image-20221016145115854.png)

## Excel导入

### 文件上传

- 可以配置文件上传多部分解析器

### 具体步骤

1. Controller

```java
@GetMapping("/import")
public void read() throws IOException, SQLException {
    String path = "C:\\Users\\BDA\\Desktop\\1.xlsx";
    TestDataListener testDataListener = new TestDataListener(testMapper);
    ExcelReader read = EasyExcel.read(path, TestDTO.class, testDataListener).build();
    ReadSheet sheet = EasyExcel.readSheet(0).build();
    read.read(sheet);
}
```

2. Listener

```java
public class TestDataListener extends AnalysisEventListener<TestDTO> {
	private TestMapper testMapper;
	private static final int BATCH_COUNT = 1000;
	private List<TestDTO> cachedDataList = ListUtils.newArrayListWithExpectedSize(BATCH_COUNT);
	public TestDataListener() {}
	public TestDataListener(TestMapper testMapper) {
		this.testMapper = testMapper;
	}

	@Override
	public void invoke(TestDTO data, AnalysisContext analysisContext) {
		cachedDataList.add(data);
		if (cachedDataList.size() >= BATCH_COUNT) {
			insertList();
			cachedDataList = ListUtils.newArrayListWithExpectedSize(BATCH_COUNT);
		}
	}
	public void insertList(){
		if (cachedDataList.size() > 0) {
			testMapper.insertList(cachedDataList);
		}
	}

	@Override
	public void doAfterAllAnalysed(AnalysisContext analysisContext) {

	}
}
```

3. sql语句

```xml
<insert id="insertList">
    insert into test2
    values
    <foreach collection="list" item="item" index="index" separator=",">
        (#{item.name,jdbcType=VARCHAR},#{item.age,jdbcType=VARCHAR})
    </foreach>
</insert>
```

## Excel导出

### Excel填充

```java
//准备模板
String template = "模板路径";
//创建工作簿对象
ExcelWrite excelWrite = EasyExcel.write("导出路径", 对象).withTemplate(template).build();
//创建工作表对象
WriteSheet sheet = EasyExcel.writeSheet().build();
//换行(解决多个数据交替执行缺失数据问题)
	//垂直填充
	FillConfig config = FillConfig.builder().forceNewRow(true).build();
	//水平填充
	FillConfig.builder().direction(WriteDirectionEnum.HORIZONATL).build();
//数据
List<FillData> data = new ArrayList<>();//list类型
HashMap<String, String> data = new HashMap<>();//map类型
//填充数据
excelWrite.fill(data, config, sheet);
//关闭流
excelWrite.finish();
```

### 文件下载

- 

## 注解

### 导出

#### @ResponseExcel()

- fileName= "Java知识日历20201101测试"
- sheetName = "同一班的同学名册"
- columnNames= {"学生姓名","学号","年龄"},classFieldNames = { "name","stuNo","age" }

### 导入

#### @RequestExcel

- 可以指定导入文件的名称，名称相同才能解析，默认是file

## 模板下载

