> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/chenzhanhai/article/details/83352953)

    日志按所在主体分为

       系统

       子系统

       模块

       子模块

   日志按严重类型分为

       信息

       警告

       错误

       致命错误

  **该类被调用示例如下**

```
LogManager logManager = new LogManager("SystemName","SubSystemName","ModuleName","SubModuleName");
   
   logManager.info("this is info message");
   logManager.warn("this is warn message");
   logManager.error("this is error message");
   logManager.fatal("this is fatal message");
```

 **该类源码如下**

```
package common;
 
import java.io.IOException;
import java.sql.Timestamp;
import java.text.SimpleDateFormat;
 
 
 
public class LogManager {
 
 // m_szSystemName
 private String m_szSystemName = "";
 public void setSystemName(String strSystemName){
  m_szSystemName = strSystemName;
 }
 public String getSystemName(){
  return m_szSystemName;
 }
 
 // m_szSubSystemName
 private String m_szSubSystemName = "";
 public void setSubSystemName(String strSubSystemName){
  m_szSubSystemName = strSubSystemName;
 }
 public String getSubSystemName(){
  return m_szSubSystemName;
 }
 
 // m_szMoudleName
 private String m_szMoudleName = "";
 public void setMoudleName(String strMoudleName){
  m_szMoudleName = strMoudleName;
 }
 public String getMoudleName(){
  return m_szMoudleName;
 }
 
 // m_szSubMoudleName
 private String m_szSubMoudleName = "";
 public void setSubMoudleName(String strSubMoudleName){
  m_szSubMoudleName = strSubMoudleName;
 }
 public String getSubMoudleName(){
  return m_szSubMoudleName;
 }
 
 // m_szLogContent
 private String m_szLogContent = "";
 public void setLogContent(String strLogContent){
  m_szLogContent = strLogContent;
 }
 public String getLogContent(){
  return m_szLogContent;
 }
  
 // 构造函数
    LogManager(){
  
 }
 LogManager(String strSubMoudleName){
  m_szSubMoudleName = strSubMoudleName;
 }
 LogManager(String strMoudleName,String strSubMoudleName){
  m_szMoudleName = strMoudleName;
  m_szSubMoudleName = strSubMoudleName;
 }
 LogManager(String strSubSystemName, String strMoudleName,String strSubMoudleName){
  m_szSubSystemName = strSubSystemName;
  m_szMoudleName = strMoudleName;
  m_szSubMoudleName = strSubMoudleName;
 }
 LogManager(String strSystemName, String strSubSystemName, String strMoudleName,String strSubMoudleName){
  m_szSystemName = strSystemName;
  m_szSubSystemName = strSubSystemName;
  m_szMoudleName = strMoudleName;
  m_szSubMoudleName = strSubMoudleName;
 }
 
 /**
  * print函数
  * 1.获取当前时间
  * 2.根据时间及内容组成打印字符串
  * 3.打印字符串
  *
  * nFailLevel:
  * 0 info
  * 1 warn
  * 2 error
  * 3 fatal
  *
  * */
 private void print(int nFailLevel){
  
  // 日期字符串
  SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS");
        String currentTime = sdf.format(System.currentTimeMillis());
  StringBuffer sb = new StringBuffer(currentTime);
  
  // 添加错误等级
  sb.append(" ");
  
  switch(nFailLevel){
  // 0 info
  case 0:
   sb.append("info ").append(" ");
   break;
  // 1 warn
  case 1:
   sb.append("warn ").append(" ");
   break;
  // 2 error
  case 2:
   sb.append("error").append(" ");
   break;
  // 3 fatal
  case 3:
   sb.append("fatal").append(" ");
   break;
  // default info
  default:
   sb.append("info ").append(" ");
  }
  
  // 组织模块名组合
  if(m_szSystemName.length() > 0){
   sb.append("[").append(m_szSystemName).append("]");
  }
  if(m_szSubSystemName.length() > 0){
   sb.append("[").append(m_szSubSystemName).append("]");
  }
  if(m_szMoudleName.length() > 0){
   sb.append("[").append(m_szMoudleName).append("]");
  }
  if(m_szSubMoudleName.length() > 0){
   sb.append("[").append(m_szSubMoudleName).append("]");
  }
  
  // 组织打印内容
  if(m_szLogContent.length() > 0){
   sb.append(" ").append(m_szLogContent);
  }
  
  // 系统窗口打印log;
  System.out.print(sb);
  // log文件打印log
  LogFileManager logFileManager = LogFileManager.getInstance();
  logFileManager.print(sb);
  // Debug窗口打印log
  DeugFrame deugFrame = DeugFrame.getInstance();
  deugFrame.print(sb); }
 
 // info function
 public void info(String strInfo){
  m_szLogContent = strInfo + "/n";
  print(0);
 }
    public void info(String strInfo, Exception eInfo){
     m_szLogContent = strInfo + "/n";
     m_szLogContent = m_szLogContent + getExceptionString(eInfo).toString();
     print(0);
 }
 
    // warn function
 public void warn(String strInfo){
  m_szLogContent = strInfo + "/n";
  print(1);
 }
    public void warn(String strInfo, Exception eInfo){
     m_szLogContent = strInfo + "/n";
     m_szLogContent = m_szLogContent + getExceptionString(eInfo).toString();
     
     print(1);
 }
   
    // error function
 public void error(String strInfo){
  m_szLogContent = strInfo + "/n";
  print(2);
 }
    public void error(String strInfo, Exception eInfo){
     m_szLogContent = strInfo + "/n";
     m_szLogContent = m_szLogContent + getExceptionString(eInfo).toString();
     print(2);
 }
   
    // fatal function
 public void fatal(String strInfo){
  m_szLogContent = strInfo + "/n";
  print(3);
 }
    public void fatal(String strInfo, Exception eInfo){
     m_szLogContent = strInfo + "/n";
     m_szLogContent = m_szLogContent + getExceptionString(eInfo).toString();
     print(3);
 }
   
    /*
     * 根据Exception获取栈中内容
     *
     * */
    private String getExceptionString(Exception eInfo){
     StringBuffer sb = new StringBuffer(eInfo.toString() + "/n");
     StackTraceElement[] steTmp = eInfo.getStackTrace();
     
     for(int i = 0; i < steTmp.length; i++){
      sb.append(steTmp[i].toString()).append("/n");
     }
     
     return sb.toString();
    }
   
   
 public static void main(String[] args) {
  try {
   LogManager logManager = new LogManager("SystemName","SubSystemName","ModuleName","SubModuleName");
   
   logManager.info("this is info message");
   logManager.warn("this is warn message");
   logManager.error("this is error message");
   logManager.fatal("this is fatal message");
   
   try{
    String sTmp = null;
    int n = sTmp.length();
   }
   catch(Exception e1){
    logManager.info("abc",e1);
    logManager.warn("def",e1);
    logManager.error("def",e1);
    logManager.fatal("def",e1);
   }
   
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
 }
}
```