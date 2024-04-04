
###### DB서버에 접속이 잘되는지 확인 필요

__NGV 경우 포팅초기에 단일 java파일을 생성하여 DB connection 테스트 진행__

***맞는 connector java 잘찾아야함. (T-story의 이상한거 읽지말고 공식 문서 확인필요)***

===포팅 막바지에 DB 연결안되서 빠꾸먹는일 최소화===


```java
//NGV 기준 (mssql 2016...) 

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class MSSQLConnectionTest {
    public static void main(String[] args) {
        String jdbcUrl = "jdbc:sqlserver://10.6.149.35:1433;databaseName=ngv_scholar";
        String username = "ngv_scholar_user1";
        String password = "NsuPass1!#";

        try {
            // JDBC 드라이버 로드
            Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");

            // 데이터베이스 연결
            Connection connection = DriverManager.getConnection(jdbcUrl, username, password);

            if (connection != null) {
                System.out.println("MSSQL에 성공적으로 연결되었습니다.");
            } else {
                System.out.println("MSSQL 연결에 실패했습니다.");
            }

            // 연결 종료
            connection.close();
        } catch (ClassNotFoundException e) {
            System.out.println("JDBC 드라이버를 찾을 수 없습니다.");
            e.printStackTrace();
        } catch (SQLException e) {
            System.out.println("MSSQL 연결 중 오류가 발생했습니다.");
            e.printStackTrace();
        }
    }
}


```