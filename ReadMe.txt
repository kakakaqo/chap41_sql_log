
 프로젝트 : chap41_sql_log

[SQL 로그 출력하기]
 - 참고 사이트 :https://admm.tistory.com/59


1. pom.xml
	<!-- SQL Log 출력 -->
	<dependency>
	    <groupId>org.bgee.log4jdbc-log4j2</groupId>
	    <artifactId>log4jdbc-log4j2-jdbc4.1</artifactId>
	    <version>1.16</version>
	</dependency>
		
	<!-- HikariCP Connection Pool -->
	<dependency>
        <groupId>com.zaxxer</groupId>
        <artifactId>HikariCP</artifactId>
        <version>2.6.3</version>
    </dependency>

 - 저장후 메이븐 업데이트

2. root-context.xml 에 log4jdbc 관련 내용 수정

	<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
   	    <property name="driverClassName" value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy"/>
	    <property name="jdbcUrl" value="jdbc:log4jdbc:oracle:thin:@127.0.0.1:1521:orcl"/>
	    <property name="username" value="board"/>
	    <property name="password" value="1234"/>
	</bean>
	
	Base-package 경로 확인

3. servlet-context.xml

	Base-package 경로 확인


4. properties 파일 추가
 - log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator

5. dao 단의 DataSoruce가 HicakariCP로 바뀜
  - javax.sql.DataSource;
  
4.logback.xml 수정(SQL 쿼리문 출력 형태 변경)
 	<!-- SQL관련 설정 -->
 	<!-- SQL문만을 로그로 남기며, PreparedStatement일 경우 관련된 argument 값으로 대체된 SQL문이 보여진다. -->
	<logger name="jdbc.sqlonly" level="DEBUG"/>    
	<!-- SQL문과 해당 SQL을 실행시키는데 수행된 시간 정보(milliseconds)를 포함한다. -->
	<logger name="jdbc.sqltiming" level="DEBUG"/>
	<!-- ResultSet을 제외한 모든 JDBC 호출 정보를 로그로 남긴다. 많은 양의 로그가 생성되므로 특별히 JDBC 문제를 추적해야 할 필요가 있는 경우를 제외하고는 사용을 권장하지 않는다. -->    
	<logger name="jdbc.audit" level="ERROR"/>    
	<!-- ResultSet을 포함한 모든 JDBC 호출 정보를 로그로 남기므로 매우 방대한 양의 로그가 생성된다. -->
	<logger name="jdbc.resultset" level="ERROR"/> 
	
5. 모든 테스트가 끝난 후에 root-context.xml에서 SQL 로그를 남기기기 위한 다음 작업 필요

	<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
   	    <property name="driverClassName" value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy"/>
	    <property name="jdbcUrl" value="jdbc:log4jdbc:oracle:thin:@127.0.0.1:1521:orcl"/>
	    <property name="username" value="board"/>
	    <property name="password" value="1234"/>
	</bean>	