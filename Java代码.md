# `Java JDBC`

##  数据库驱动类和链接URL字符串

| 数据库 | Driver | URL |
|-------|--------|-----|
| `Oracle` | `oracle.jdbc.driver.OracleDriver` | `jdbc:oracle:thin:@loaclhost:port:dbname` |
| `MySQL` | `com.mysql.jdbc.Driver`| `jdbc:mysql://localhost:port/dbname` |
| `SQL2008` | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | `jdbc:sqlserver://localhost:port;databaseName=dbname`|


# BaseDB
```java
package util;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class BaseDB {
	/*
	 * 数据库连接信息
	 */
	private static final String driver = "com.mysql.cj.jdbc.Driver";
	private static final String url = "jdbc:mysql://localhost:3306/testdb?serverTimezone=UTC";
	private static final String user = "root";
	private static final String pwd = "root";

	/**
	 * 加载驱动获取连接
	 * 
	 * @return
	 */
	private static Connection getConnection() {
		Connection conn = null;
		try {
			Class.forName(driver);
			conn = DriverManager.getConnection(url, user, pwd);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return conn;
	}

	/**
	 * 增删改
	 * 
	 * @param sql
	 * @param objs参数列表
	 * @return int受影响行数
	 */
	public static int update(String sql, Object... objs) {
		int i = 0;
		try {
			// 加载驱动获取连接
			Connection conn = getConnection();

			PreparedStatement pstmt = conn.prepareStatement(sql);

			// 设置参数
			setParamters(pstmt, objs);

			i = pstmt.executeUpdate();
			close(conn, pstmt, null);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return i;
	}

	/**
	 * 查询
	 * 
	 * @param sql要执行的查询语句
	 * @param objs参数列表
	 * @return List<Map<String, Object>>查询结果map存一行，list存多行
	 */
	public static List<Map<String, Object>> query(String sql, Object... objs) {
		// 保存查询出来结果信息：所有行
		List<Map<String, Object>> list = new ArrayList<Map<String, Object>>();
		try {
			Connection conn = getConnection();
			PreparedStatement pstmt = conn.prepareStatement(sql);
			// 设置参数
			setParamters(pstmt, objs);

			ResultSet rs = pstmt.executeQuery();
			ResultSetMetaData metaData = rs.getMetaData();
			// 获取查询结果的列数
			int columnCount = metaData.getColumnCount();
			// 循环每一行
			while (rs.next()) {
				// 变量保存一行(行保存每一列的值) 集合 Map<列名，列值>
				Map<String, Object> row = new HashMap<String, Object>();

				// 循环当前行的每一列
				for (int i = 1; i <= columnCount; i++) {
					Object columnValue = rs.getObject(i);// 列值
					String columnName = metaData.getColumnName(i);// 列名
					row.put(columnName, columnValue);
				}

				// 将当前行添加到集合中返回
				list.add(row);
			}

			close(conn, pstmt, rs);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return list;
	}

	/**
	 * 参数赋值
	 * 
	 * @param pstmt
	 * @param objs参数列表
	 */
	private static void setParamters(PreparedStatement pstmt, Object... objs) {
		try {
			if (null != objs) {
				for (int i = 0; i < objs.length; i++) {
					pstmt.setObject(i + 1, objs[i]);
				}
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	/**
	 * 关闭连接
	 * 
	 * @param conn
	 * @param pstmt
	 * @param rs
	 */
	private static void close(Connection conn, PreparedStatement pstmt, ResultSet rs) {
		try {
			if (null != rs)
				rs.close();
			if (null != pstmt)
				pstmt.close();
			if (null != conn)
				conn.close();
		} catch (Exception e) {
			e.printStackTrace();
		}

	}
}

```


# 日期格式化
```java
public class DataTest{
    public static void main(String[] args){
        
        
        /**
              G 年代标志符
              y 年
              M 月
              d 日
              h 时 在上午或下午 (1~12)
              H 时 在一天中 (0~23)
              m 分
              s 秒
              S 毫秒
              E 星期
              D 一年中的第几天
              F 一月中第几个星期几
              w 一年中第几个星期
              W 一月中第几个星期
              a 上午 / 下午 标记符
              k 时 在一天中 (1~24)
              K 时 在上午或下午 (0~11)
              z 时区
        */
        // 方法一
        Date date = new Date();
        SimpleDateFormat DateFormat = new SimpleDateFormat( "yyyy-MM-dd HH:mm:ss S" );
        DateFormat.format(date)     // 返回 字符串 里面 规定的 内容
        
        // 方法二
        DateFormat.getDateInstance().format(date)    // 返回 年月日
        DateFormat.getDateTimeInstance().format(date)     // 返回 年月日 和 时间
        
        
        
    }
}
```


# 文件上传
```java
try {
			//创建上传对象
			SmartUpload smart=new SmartUpload();
			//对象数据初始化
			smart.initialize(this.getServletConfig(), request,response);
			smart.setCharset("utf-8");
			//允许文件列表
			smart.setAllowedFilesList("txt");
			//拒绝的文件列表
			smart.setDeniedFilesList("png");
			smart.setMaxFileSize(200);
			//上传文件
			smart.upload();
			//获取文件上传模式对应的客户端请求对象
			Request sreq=smart.getRequest();
			
			//String strName=request.getParameter("name");
			//String strPrice=request.getParameter("price");
			
			String strName=sreq.getParameter("name");
			String strPrice=sreq.getParameter("price");
			
			System.out.println("商品名称:"+strName);
			System.out.println("商品价格:"+strPrice);
			
			Files smartFiles=smart.getFiles();
			int count=smartFiles.getCount();
			if(smartFiles.getCount()>0){
				File smartFile=smartFiles.getFile(0);
				System.out.println("=================");
				System.out.println(smartFile);
				String path=this.getServletContext().getRealPath("/");
				System.out.println(path);
				//smartFile.saveAs("/upload/"+smartFile.getFileName());
				//smart.save("/upload");
				
				for(int i=0;i<count;i++){
					//File f=smart.getFiles().getFile(i);
					//file.save 要存具体的文件名
					//f.saveAs("/upload/"+java.util.UUID.randomUUID()+"."+f.getFileExt());
					//smart.save 只能存目录，会自动保存原有上传的文件名
					smart.save("/upload");
				}
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		}
```

js 上传显示图片

```html
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="Test1.aspx.cs" Inherits="Proj_.Test1" %>

<!DOCTYPE html>

<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    <title></title>
    <script type="text/ecmascript">
        function xg()
        {
            var reader = new FileReader();

            reader.onload = function (evt) {
                img.src = evt.target.result;
            }

            reader.readAsDataURL(file.files[0]);
            alert(123);
        }

    </script>
</head>
<body>
    <form id="form1" runat="server">
        <asp:FileUpload ID="file" runat="server" onchange="xg()" /><br />
        <img src="" id="img" width="200" height="200" />
    </form>
</body>
</html>

```


# 输入
```java
public class test{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        sc.nextLine();
    }
}

```


