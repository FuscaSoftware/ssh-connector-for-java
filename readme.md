## SSH Connector for JAVA

#painless connection to shh with java


Little Example...
**doSshTunnel()** create the ssh tunneling and execute the port forwarding
**TestRun()** collect all needed IP's, String's and creditals and execute the mySQl connection
 

```java

import java.io.FileOutputStream;
import java.io.PrintStream;
import java.sql.*;
import java.util.Iterator;
import java.util.LinkedHashMap;
import java.util.LinkedList;
import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.Scanner;
import org.json.simple.parser.ContainerFactory;
import org.json.simple.parser.JSONParser;

import com.skalski.ssh.*

import java.sql.DriverManager;
import java.sql.Statement;
import java.text.ParseException;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Test {
	// JDBC driver name and database URL
	static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";
	static final String DB_URL = "jdbc:mysql://127.0.0.1:3306/testtable";

	// Database credentials
	static final String USER = "root";
	static final String PASS = "";

	private static void doSshTunnel(String strSshUser, String strSshPassword, String strSshHost, int nSshPort,
		String strRemoteHost, int nLocalPort, int nRemotePort) throws SSHCallException {
		SSHCall SSHCall = new SSHCall();
		Session session = SSHCall.getSession(strSshUser, strSshHost, 22);
		session.setPassword(strSshPassword);

		final Properties config = new Properties();
		config.put("StrictHostKeyChecking", "no");
		session.setConfig(config);

		session.connect();
		session.setPortForwardingL(nLocalPort, strRemoteHost, nRemotePort);
	}

	public static void TestRun() throws ParseException {
		Connection conn = null;
		Statement stmt = null;

		String strSshUser = "username";
		String strSshPassword = "passwort";
		String strSshHost = "123.0.0.123";
		int nSshPort = 22;
		int nLocalPort = 3306;
		int nRemotePort = 3306;
		String strRemoteHost = "127.0.0.1";
		System.out.println("Connecting to SHH...");
		try {
				Test.doSshTunnel(strSshUser, strSshPassword, strSshHost, nSshPort, strRemoteHost, nLocalPort,nRemotePort);
			} catch (Exception e) {
				System.exit(0);
			}

			Class.forName("com.mysql.jdbc.Driver");
			System.out.println("Connecting to database...");
			conn = DriverManager.getConnection(DB_URL, USER, PASS);

			stmt = conn.createStatement();
			String sql;
			sql = "select * from testtable;";
			ResultSet rs = stmt.executeQuery(sql);
			System.exit(0);
		}
		
		public static void main(String[] args) {
		try {
			TestRun();
		} catch (ParseException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

	}
}

```
