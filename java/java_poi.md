
# 一、POI介绍

Apache POI 是用Java编写的免费开源的跨平台的 Java API，Apache POI提供API给Java程式对Microsoft Office格式档案读和写的功能。POI为“Poor Obfuscation Implementation”的首字母缩写，意为“可怜的模糊实现”。

Apache POI 是创建和维护操作各种符合Office Open XML（OOXML）标准和微软的OLE 2复合文档格式（OLE2）的Java API。用它可以使用Java读取和创建,修改MS Excel文件.而且,还可以使用Java读取和创建MS Word和MSPowerPoint文件。Apache POI 提供Java操作Excel解决方案（适用于Excel97-2008）。

此API组件的列表如下。

POIFS (较差混淆技术实现文件系统) : 此组件是所有其他POI元件的基本因素。它被用来明确地读取不同的文件。

HSSF (可怕的电子表格格式) : 它被用来读取和写入MS-Excel文件的xls格式。

XSSF (XML格式) : 它是用于MS-Excel中XLSX文件格式。

HPSF (可怕的属性设置格式) : 它用来提取MS-Office文件属性设置。

HWPF (可怕的字处理器格式) : 它是用来读取和写入MS-Word的文档扩展名的文件。

XWPF (XML字处理器格式) : 它是用来读取和写入MS-Word的docx扩展名的文件。

HSLF (可怕的幻灯片版式格式) : 它是用于读取，创建和编辑PowerPoint演示文稿。

HDGF (可怕的图表格式) : 它包含类和方法为MS-Visio的二进制文件。

HPBF (可怕的出版商格式) : 它被用来读取和写入MS-Publisher文件。

## 二、Java POI 解析Excel表格

### 2.1 引入依赖：

```xml
<dependency>
 <groupId>org.apache.poi</groupId>
   <artifactId>poi</artifactId>
    <version>4.0.1</version>
</dependency>

<dependency>
  <groupId>org.apache.poi</groupId>
  <artifactId>poi-ooxml</artifactId>
  <version>4.0.1</version>
</dependency>

```

### 2.2 读取Excel文件

POI的excel类主要有四个属性，Workbook(工作表)，Sheet(表单)，Row(行), Cell(单元格)

读取Excel的思路是按照Workbook，Sheet，Row，Cell一层一层往下读取。

* ***初始化Workbook**

```java
private Workbook getReadWorkBookType(String filePath) throws BusinessException {
        //xls-2003, xlsx-2007
        FileInputStream is = null;

        try {
            is = new FileInputStream(filePath);
            if (filePath.toLowerCase().endsWith("xlsx")) {
                return new XSSFWorkbook(is);
            } else if (filePath.toLowerCase().endsWith("xls")) {
                return new HSSFWorkbook(is);
            } else {
              //  抛出自定义的业务异常
                throw OnlinePayErrorCode.EXCEL_ANALYZE_ERROR.convertToException("excel格式文件错误");
            }
        } catch (IOException e) {
          //  抛出自定义的业务异常
            throw OnlinePayErrorCode.EXCEL_ANALYZE_ERROR.convertToException(e.getMessage());
        } finally {
            IOUtils.closeQuietly(is);
        }
  }
```


* **解析Excel文件**
* 
因为我的excel只有一页sheet，所以直接读取第0页sheet(workbook.getSheetAt(0))，如果有多个可以自行顺序循环读取；

并且我只有一列数据，所以每次只读取第0列的数据(row.getCell(0))，如果有多个可以依次循环读取。

把excel文件里的数据读取放入一个List<String>中

```java
public List<String> readExcel(String sourceFilePath) throws BusinessException {
        Workbook workbook = null;

        try {
            workbook = getReadWorkBookType(sourceFilePath);
            List<String> contents = Lists.newArrayList();

            //获取第一个sheet
            Sheet sheet = workbook.getSheetAt(0);
            //第0行是表名，忽略，从第二行开始读取
            for (int rowNum = 1; rowNum <= sheet.getLastRowNum(); rowNum++) {
                Row row = sheet.getRow(rowNum);
                Cell cell = row.getCell(0);
                contents.add(getCellStringVal(cell).trim());
            }
            return contents;
        } finally {
            IOUtils.closeQuietly(workbook);
        }
    }
    ```
如果excel中的数据是数字，会发现java中对应的变成了科学计数法的，

所以在获取值的时候就要做一些特殊处理，这样就能保证获取的值是我想要的值。

```java
private String getCellStringVal(Cell cell) {
        CellType cellType = cell.getCellTypeEnum();
        switch (cellType) {
            case NUMERIC:
                return cell.getStringCellValue();
            case STRING:
                return cell.getStringCellValue();
            case BOOLEAN:
                return String.valueOf(cell.getBooleanCellValue());
            case FORMULA:
                return cell.getCellFormula();
            case BLANK:
                return "";
            case ERROR:
                return String.valueOf(cell.getErrorCellValue());
            default:
                return StringUtils.EMPTY;
        }
    }
```