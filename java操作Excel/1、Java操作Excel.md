# 1、Java操作Excel

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>java-notes-code</artifactId>
        <groupId>org.example</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>java-excel</artifactId>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>6</source>
                    <target>6</target>
                </configuration>
            </plugin>
        </plugins>
    </build>

    <dependencies>
<!--        <dependency>-->
<!--            <groupId>org.apache.poi</groupId>-->
<!--            <artifactId>poi</artifactId>-->
<!--            <version>4.1.2</version>-->
<!--        </dependency>-->
<!--        <dependency>-->
<!--            <groupId>org.apache.poi</groupId>-->
<!--            <artifactId>poi-ooxml</artifactId>-->
<!--            <version>4.1.2</version>-->
<!--        </dependency>-->

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>easyexcel</artifactId>
            <version>2.2.6</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.14</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.74</version>
        </dependency>


        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.7.25</version>
            <scope>compile</scope>
        </dependency>
    </dependencies>

</project>

```

## 方式一:`apache poi`


```java
package com.feige.poi;

import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;
import org.apache.poi.xssf.streaming.SXSSFWorkbook;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import org.junit.Test;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;

public class ExcelWriteDemo {
    //文件输出路径
    public static final File file = new File("E:\\project\\java-project\\java-notes-code\\java-excel");
    @Test
    public void test03write() throws IOException {
        long start = System.currentTimeMillis();
        OutputStream os = null;
        try {
            //输出流
            os = new FileOutputStream(file + "\\feige.xls");
            //创建一个工作薄03版本65536行
            Workbook workbook = new HSSFWorkbook();
            //创建一个工作表
            Sheet sheet = workbook.createSheet("feige");
            for (int rowNum = 0; rowNum < 65536; rowNum++) {
                //创建行
                Row row = sheet.createRow(rowNum);
                for (int cellNum = 0; cellNum < 10; cellNum++) {
                    //创建列
                    Cell cell = row.createCell(cellNum);
                    //给列赋值
                    cell.setCellValue(cellNum);
                }
            }
            //写入输出流
            workbook.write(os);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (os != null) {
                os.close();
            }
        }
        long end = System.currentTimeMillis();
        //time:2.864s
        System.out.println("time:" + (double)(end - start) / 1000 + "s");
    }
    @Test
    public void test07write() throws IOException {
        long start = System.currentTimeMillis();
        OutputStream os = null;
        try {
            //输出流
            os = new FileOutputStream(file + "\\feige.xlsx");
            //创建一个工作薄07版本理论上无限行
            Workbook workbook = new XSSFWorkbook();
            //创建一个工作表
            Sheet sheet = workbook.createSheet("feige");
            for (int rowNum = 0; rowNum < 65536; rowNum++) {
                //创建行
                Row row = sheet.createRow(rowNum);
                for (int cellNum = 0; cellNum < 10; cellNum++) {
                    //创建列
                    Cell cell = row.createCell(cellNum);
                    //给列赋值
                    cell.setCellValue(cellNum);
                }
            }
            //写入输出流
            workbook.write(os);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (os != null) {
                os.close();
            }
        }
        long end = System.currentTimeMillis();
        //time:7.168s
        System.out.println("time:" + (double)(end - start) / 1000 + "s");
    }

    @Test
    public void test07writeBigData() throws IOException {
        long start = System.currentTimeMillis();
        OutputStream os = null;
        Workbook workbook = null;
        try {
            //输出流
            os = new FileOutputStream(file + "\\feige.xlsx");
            //创建一个工作薄07版本理论上无限行
            workbook = new SXSSFWorkbook();

            //创建一个工作表
            Sheet sheet = workbook.createSheet("feige");
            for (int rowNum = 0; rowNum < 65536; rowNum++) {
                //创建行
                Row row = sheet.createRow(rowNum);
                for (int cellNum = 0; cellNum < 10; cellNum++) {
                    //创建列
                    Cell cell = row.createCell(cellNum);
                    //给列赋值
                    cell.setCellValue(cellNum);
                }
            }
            //写入输出流
            workbook.write(os);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (os != null) {
                os.close();
            }
            if (workbook != null) {
                ((SXSSFWorkbook) workbook).dispose();
            }
        }
        long end = System.currentTimeMillis();
        //time:2.139s
        System.out.println("time:" + (double)(end - start) / 1000 + "s");
    }
}
```

```java
package com.feige.poi;

import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;
import org.apache.poi.xssf.streaming.SXSSFWorkbook;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import org.junit.Test;

import java.io.*;

public class ExcelReadDemo {
    //文件输出路径
    public static final File file = new File("E:\\project\\java-project\\java-notes-code\\java-excel");
    @Test
    public void test03read() throws IOException {
        long start = System.currentTimeMillis();
        InputStream is = null;
        try {
            is = new FileInputStream(file + "\\feige.xls");
            Workbook workbook = new HSSFWorkbook(is);
            Sheet sheet = workbook.getSheet("feige");
            int rowCount = sheet.getPhysicalNumberOfRows();
            for (int rowNum = 0; rowNum < rowCount; rowNum++) {
                Row row = sheet.getRow(rowNum);
                int cellCount = row.getPhysicalNumberOfCells();
                for (int cellNum = 0; cellNum < cellCount; cellNum++) {
                    Cell cell = row.getCell(cellNum);
                    double value = cell.getNumericCellValue();
                    System.out.print(value + " | ");
                }
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (is != null) {
                is.close();
            }
        }
        long end = System.currentTimeMillis();
        //time:2.227s
        System.out.println("time:" + (double)(end - start) / 1000 + "s");
    }

    @Test
    public void test07read() throws IOException {
        long start = System.currentTimeMillis();
        InputStream is = null;
        try {
            is = new FileInputStream(file + "\\feige.xlsx");
            Workbook workbook = new XSSFWorkbook(is);
            Sheet sheet = workbook.getSheet("feige");
            int rowCount = sheet.getPhysicalNumberOfRows();
            for (int rowNum = 0; rowNum < rowCount; rowNum++) {
                Row row = sheet.getRow(rowNum);
                int cellCount = row.getPhysicalNumberOfCells();
                for (int cellNum = 0; cellNum < cellCount; cellNum++) {
                    Cell cell = row.getCell(cellNum);
                    double value = cell.getNumericCellValue();
                    System.out.print(value + " | ");
                }
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (is != null) {
                is.close();
            }
        }
        long end = System.currentTimeMillis();
        //time:7.379s
        System.out.println("time:" + (double)(end - start) / 1000 + "s");
    }

    @Test
    public void test07readBigData() throws IOException {
        long start = System.currentTimeMillis();
        InputStream is = null;
        try {
            is = new FileInputStream(file + "\\feige.xlsx");
            Workbook workbook = new SXSSFWorkbook(new XSSFWorkbook(is));
            Sheet sheet = workbook.getSheet("feige");
            int rowCount = sheet.getPhysicalNumberOfRows();
            for (int rowNum = 0; rowNum < rowCount; rowNum++) {
                Row row = sheet.getRow(rowNum);
                int cellCount = row.getPhysicalNumberOfCells();
                for (int cellNum = 0; cellNum < cellCount; cellNum++) {
                    Cell cell = row.getCell(cellNum);
                    double value = cell.getNumericCellValue();
                    System.out.print(value + " | ");
                }
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (is != null) {
                is.close();
            }
        }
        long end = System.currentTimeMillis();
        //time:7.379s
        System.out.println("time:" + (double)(end - start) / 1000 + "s");
    }
}
```

```java
package com.feige.poi;

import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;
import org.apache.poi.xssf.streaming.SXSSFWorkbook;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import org.junit.Test;

import java.io.*;

public class ExcelReadDemo {
    //文件输出路径
    public static final File file = new File("E:\\project\\java-project\\java-notes-code\\java-excel");
    @Test
    public void test03read() throws IOException {
        long start = System.currentTimeMillis();
        InputStream is = null;
        try {
            is = new FileInputStream(file + "\\feige.xls");
            Workbook workbook = new HSSFWorkbook(is);
            Sheet sheet = workbook.getSheet("feige");
            int rowCount = sheet.getPhysicalNumberOfRows();
            for (int rowNum = 0; rowNum < rowCount; rowNum++) {
                Row row = sheet.getRow(rowNum);
                int cellCount = row.getPhysicalNumberOfCells();
                for (int cellNum = 0; cellNum < cellCount; cellNum++) {
                    Cell cell = row.getCell(cellNum);
                    double value = cell.getNumericCellValue();
                    System.out.print(value + " | ");
                }
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (is != null) {
                is.close();
            }
        }
        long end = System.currentTimeMillis();
        //time:2.227s
        System.out.println("time:" + (double)(end - start) / 1000 + "s");
    }

    @Test
    public void test07read() throws IOException {
        long start = System.currentTimeMillis();
        InputStream is = null;
        try {
            is = new FileInputStream(file + "\\feige.xlsx");
            Workbook workbook = new XSSFWorkbook(is);
            Sheet sheet = workbook.getSheet("feige");
            int rowCount = sheet.getPhysicalNumberOfRows();
            for (int rowNum = 0; rowNum < rowCount; rowNum++) {
                Row row = sheet.getRow(rowNum);
                int cellCount = row.getPhysicalNumberOfCells();
                for (int cellNum = 0; cellNum < cellCount; cellNum++) {
                    Cell cell = row.getCell(cellNum);
                    double value = cell.getNumericCellValue();
                    System.out.print(value + " | ");
                }
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (is != null) {
                is.close();
            }
        }
        long end = System.currentTimeMillis();
        //time:7.379s
        System.out.println("time:" + (double)(end - start) / 1000 + "s");
    }

    @Test
    public void test07readBigData() throws IOException {
        long start = System.currentTimeMillis();
        InputStream is = null;
        try {
            is = new FileInputStream(file + "\\feige.xlsx");
            Workbook workbook = new SXSSFWorkbook(new XSSFWorkbook(is));
            Sheet sheet = workbook.getSheet("feige");
            int rowCount = sheet.getPhysicalNumberOfRows();
            for (int rowNum = 0; rowNum < rowCount; rowNum++) {
                Row row = sheet.getRow(rowNum);
                int cellCount = row.getPhysicalNumberOfCells();
                for (int cellNum = 0; cellNum < cellCount; cellNum++) {
                    Cell cell = row.getCell(cellNum);
                    double value = cell.getNumericCellValue();
                    System.out.print(value + " | ");
                }
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (is != null) {
                is.close();
            }
        }
        long end = System.currentTimeMillis();
        //time:7.379s
        System.out.println("time:" + (double)(end - start) / 1000 + "s");
    }
}
```

## 方式二:`alibaba easyexcel`


```java
package com.feige.easyexcel;

import com.alibaba.excel.EasyExcel;
import org.junit.Test;

import java.io.File;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;


public class EasyExcelDemo {
    public static final File file = new File("E:\\project\\java-project\\java-notes-code\\java-excel");


    private List<DemoData> data() {
        List<DemoData> list = new ArrayList<DemoData>();
        for (int i = 0; i < 10; i++) {
            DemoData data = new DemoData();
            data.setString("字符串" + i);
            data.setDate(new Date());
            data.setDoubleData(0.56);
            list.add(data);
        }
        return list;
    }

    /**
     * 最简单的写
     * <p>1. 创建excel对应的实体对象 参照{@link DemoData}
     * <p>2. 直接写即可
     */
    @Test
    public void simpleWrite() {
        // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
        // 如果这里想使用03 则 传入excelType参数即可
        EasyExcel.write(file + "\\feige.xlsx", DemoData.class).sheet("模板").doWrite(data());
    }


    /**
     * 指定列的下标或者列名
     *
     * <p>1. 创建excel对应的实体对象,并使用注解. 参照{@link IndexOrNameData}
     * <p>2. 由于默认一行行的读取excel，所以需要创建excel一行一行的回调监听器，参照{@link }
     * <p>3. 直接读即可
     */
    @Test
    public void indexOrNameRead() {
        // 这里默认读取第一个sheet
        EasyExcel.read(file + "\\feige.xlsx", DemoData.class, new DemoDataListener()).sheet().doRead();
    }


}
```

```java
package com.feige.easyexcel;

import com.alibaba.excel.context.AnalysisContext;
import com.alibaba.excel.event.AnalysisEventListener;
import com.alibaba.fastjson.JSON;
import lombok.extern.slf4j.Slf4j;

import java.util.ArrayList;
import java.util.List;

// 有个很重要的点 DemoDataListener 不能被spring管理，要每次读取excel都要new,然后里面用到spring可以构造方法传进去
@Slf4j
public class DemoDataListener extends AnalysisEventListener<DemoData> {
    /**
     * 每隔5条存储数据库，实际使用中可以3000条，然后清理list ，方便内存回收
     */
    private static final int BATCH_COUNT = 5;
    List<DemoData> list = new ArrayList<DemoData>();
    /**
     * 假设这个是一个DAO，当然有业务逻辑这个也可以是一个service。当然如果不用存储这个对象没用。
     */
    private DemoDAO demoDAO;
    public DemoDataListener() {
        // 这里是demo，所以随便new一个。实际使用如果到了spring,请使用下面的有参构造函数
        demoDAO = new DemoDAO();
    }
    /**
     * 如果使用了spring,请使用这个构造方法。每次创建Listener的时候需要把spring管理的类传进来
     *
     * @param demoDAO
     */
    public DemoDataListener(DemoDAO demoDAO) {
        this.demoDAO = demoDAO;
    }
    /**
     * 这个每一条数据解析都会来调用
     *
     * @param data
     *            one row value. Is is same as {@link AnalysisContext#readRowHolder()}
     * @param context
     */
    @Override
    public void invoke(DemoData data, AnalysisContext context) {
        log.info("解析到一条数据:{}", JSON.toJSONString(data));
        list.add(data);
        // 达到BATCH_COUNT了，需要去存储一次数据库，防止数据几万条数据在内存，容易OOM
        if (list.size() >= BATCH_COUNT) {
            saveData();
            // 存储完成清理 list
            list.clear();
        }
    }
    /**
     * 所有数据解析完成了 都会来调用
     *
     * @param context
     */
    @Override
    public void doAfterAllAnalysed(AnalysisContext context) {
        // 这里也要保存数据，确保最后遗留的数据也存储到数据库
        saveData();
        log.info("所有数据解析完成！");
    }
    /**
     * 加上存储数据库
     */
    private void saveData() {
        log.info("{}条数据，开始存储数据库！", list.size());
        demoDAO.save(list);
        log.info("存储数据库成功！");
    }
}
```

```java
package com.feige.easyexcel;

import com.alibaba.excel.annotation.ExcelIgnore;
import com.alibaba.excel.annotation.ExcelProperty;

import java.util.Date;


public class DemoData {
    @ExcelProperty("字符串标题")
    private String string;
    @ExcelProperty("日期标题")
    private Date date;
    @ExcelProperty("数字标题")
    private Double doubleData;
    /**
     * 忽略这个字段
     */
    @ExcelIgnore
    private String ignore;

    public String getString() {
        return string;
    }

    public void setString(String string) {
        this.string = string;
    }

    public Date getDate() {
        return date;
    }

    public void setDate(Date date) {
        this.date = date;
    }

    public Double getDoubleData() {
        return doubleData;
    }

    public void setDoubleData(Double doubleData) {
        this.doubleData = doubleData;
    }

    public String getIgnore() {
        return ignore;
    }

    public void setIgnore(String ignore) {
        this.ignore = ignore;
    }

    @Override
    public String toString() {
        return "DemoData{" +
                "string='" + string + '\'' +
                ", date=" + date +
                ", doubleData=" + doubleData +
                ", ignore='" + ignore + '\'' +
                '}';
    }
}
```

```java
package com.feige.easyexcel;

import java.util.List;

/**
 * 假设这个是你的DAO存储。当然还要这个类让spring管理，当然你不用需要存储，也不需要这个类。
 **/
public class DemoDAO {
    public void save(List<DemoData> list) {
        // 如果是mybatis,尽量别直接调用多次insert,自己写一个mapper里面新增一个方法batchInsert,所有数据一次性插入
        list.forEach(System.out::println);
    }
}
```