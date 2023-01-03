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

### 下拉框

```java
public void template(HttpServletResponse response) throws IOException {
    String fileName = "人员导入模板.xls";

    WriteCellStyle headWriteCellStyle = new WriteCellStyle();
    // 设置背景颜色
    headWriteCellStyle.setFillForegroundColor(IndexedColors.WHITE.getIndex());
    // 设置头字体
    WriteFont headWriteFont = new WriteFont();
    headWriteFont.setFontHeightInPoints((short)14);
    // 字体加粗
    headWriteFont.setBold(true);
    headWriteCellStyle.setWriteFont(headWriteFont);
    // 设置头居中
    headWriteCellStyle.setHorizontalAlignment(HorizontalAlignment.CENTER);
    // 内容策略
    WriteCellStyle contentWriteCellStyle = new WriteCellStyle();
    // 设置内容字体
    WriteFont contentWriteFont = new WriteFont();
    contentWriteFont.setFontHeightInPoints((short)12);
    contentWriteFont.setFontName("宋体");
    contentWriteCellStyle.setWriteFont(contentWriteFont);
    // 设置 水平居中
    contentWriteCellStyle.setHorizontalAlignment(HorizontalAlignment.CENTER);
    // 设置 垂直居中
    contentWriteCellStyle.setVerticalAlignment(VerticalAlignment.CENTER);
    // 设置单元格格式为 文本
    DataFormatData dataFormatData = new DataFormatData();
    dataFormatData.setIndex((short)49);
    contentWriteCellStyle.setDataFormatData(dataFormatData);

    HorizontalCellStyleStrategy horizontalCellStyleStrategy =
        new HorizontalCellStyleStrategy(headWriteCellStyle, contentWriteCellStyle);

    //示例数据
    List<HjUserExportExcelVO> example = this.list(Wrappers.<HjUserInfoDTO>query().last("limit 3")).stream().map(m ->
                                                                                                                getHjUserInfoById(m.getId())
                                                                                                               ).collect(Collectors.toList()).stream().map(m -> {
        HjUserExportExcelVO vo = new HjUserExportExcelVO();
        BeanUtil.copyProperties(m, vo);
        vo.setCompanys(m.getCompany().getName());
        vo.setDepts(m.getDept().get(0).getName());
        return vo;
    }).collect(Collectors.toList());
    response.setCharacterEncoding("UTF-8");
    response.setHeader("content-Type", "application/vnd.ms-excel");
    response.setHeader("Content-Disposition", "attachment;filename=" + URLEncoder.encode(fileName, "UTF-8"));

    // 设置表名，引脚名，文件格式，list数据
    EasyExcel.write(response.getOutputStream(), HjUserExportExcelVO.class)
        .registerWriteHandler(horizontalCellStyleStrategy)
        .registerWriteHandler(new SpinnerWriteHandler())
        .sheet("模板")
        .doWrite(example);
}
```

```java
@Component
@NoArgsConstructor
public class SpinnerWriteHandler implements SheetWriteHandler {

    private static SysDeptService sysDeptService;
    private static SysDictServiceImpl sysDictService;

    @Autowired
    public void init(SysDeptService sysDeptService, SysDictServiceImpl sysDictService){
        SpinnerWriteHandler.sysDeptService = sysDeptService;
        SpinnerWriteHandler.sysDictService = sysDictService;
    }

    @Override
    public void afterSheetCreate(WriteWorkbookHolder writeWorkbookHolder, WriteSheetHolder writeSheetHolder) {
        //部门数据
        List<String> dept = sysDeptService.getDepts();
        String[] depts = dept.toArray(new String[dept.size()]);
        List<String> company = sysDeptService.getCompanys();
        String[] companys =  company.toArray(new String[company.size()]);
        Map<Integer, String[]> mapDropDown = new HashMap<>();

        //员工属性
        List<Dict> userType = sysDictService.getByType("user_type");
        String[] userTypes = userType.stream().map(Dict::getValue).collect(Collectors.toList()).toArray(new String[userType.size()]);

        //证件类型
        List<Dict> uidType = sysDictService.getByType("id_type");
        String[] uidTypes = uidType.stream().map(Dict::getValue).collect(Collectors.toList()).toArray(new String[uidType.size()]);

        //政治面貌
        List<Dict> upolotical = sysDictService.getByType("politic_countenance");
        String[] upoloticals = upolotical.stream().map(Dict::getValue).collect(Collectors.toList()).toArray(new String[upolotical.size()]);

        //生育状况
        List<Dict> ureproductive = sysDictService.getByType("reproductive");
        String[] ureproductives = ureproductive.stream().map(Dict::getValue).collect(Collectors.toList()).toArray(new String[ureproductive.size()]);

        //婚姻状况
        List<Dict> umarital = sysDictService.getByType("marital");
        String[] umaritals = umarital.stream().map(Dict::getValue).collect(Collectors.toList()).toArray(new String[umarital.size()]);

        //合同类型
        List<Dict> ctype = sysDictService.getByType("contract_type");
        String[] ctypes = ctype.stream().map(Dict::getValue).collect(Collectors.toList()).toArray(new String[ctype.size()]);

        //学历类型
        List<Dict> etype = sysDictService.getByType("edu_type");
        String[] etypes = etype.stream().map(Dict::getValue).collect(Collectors.toList()).toArray(new String[etype.size()]);

        //员工状态
        List<Dict> userState = sysDictService.getByType("user_state");
        String[] userStates = userState.stream().map(Dict::getValue).collect(Collectors.toList()).toArray(new String[userState.size()]);

        //职称级别
        List<Dict> rankLevel = sysDictService.getByType("rank_level");
        String[] rankLevels = rankLevel.stream().map(Dict::getValue).collect(Collectors.toList()).toArray(new String[rankLevel.size()]);


        // 这里的key值 对应导出列的顺序 从0开始
        mapDropDown.put(0, companys);
        mapDropDown.put(1, depts);
        mapDropDown.put(5, userTypes);
        mapDropDown.put(6, uidTypes);
        mapDropDown.put(11, upoloticals);
        mapDropDown.put(12, ureproductives);
        mapDropDown.put(13, umaritals);
        mapDropDown.put(14, ctypes);
        mapDropDown.put(17, etypes);
        mapDropDown.put(23, userStates);
        mapDropDown.put(25, rankLevels);
        Sheet sheet = writeSheetHolder.getSheet();
        /// 开始设置下拉框
        DataValidationHelper helper = sheet.getDataValidationHelper();// 设置下拉框
        for (Map.Entry<Integer, String[]> entry : mapDropDown.entrySet())
        {
            /*** 起始行、终止行、起始列、终止列 **/
            CellRangeAddressList addressList = new CellRangeAddressList(1, 1000, entry.getKey(), entry.getKey());
            /*** 设置下拉框数据 **/
            DataValidationConstraint constraint = helper.createExplicitListConstraint(entry.getValue());
            DataValidation dataValidation = helper.createValidation(constraint, addressList);
            /*** 处理Excel兼容性问题 **/
            if (dataValidation instanceof XSSFDataValidation)
            {
                dataValidation.setSuppressDropDownArrow(true);
                dataValidation.setShowErrorBox(true);
            }
            else
            {
                dataValidation.setSuppressDropDownArrow(false);
            }
            sheet.addValidationData(dataValidation);
        }
    }
}
```

