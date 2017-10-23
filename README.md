<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:p="http://www.springframework.org/schema/p"
        xmlns:context="http://www.springframework.org/schema/context"
        xmlns:aop="http://www.springframework.org/schema/aop"
        xmlns:mvc="http://www.springframework.org/schema/mvc"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
                http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
                http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd">

    <context:component-scan base-package="egovframework,net,kr">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Service"/>
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Repository"/>
    </context:component-scan>

    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
        <property name="webBindingInitializer">
            <bean class="net.dangdang.cmn.web.EgovBindingInitializer"/>
        </property>
    </bean>
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping">
        <property name="interceptors">
            <list>
                <ref bean="localeChangeInterceptor" />
            </list>
        </property>
    </bean>
    
    <bean id="localeResolver" class="org.springframework.web.servlet.i18n.SessionLocaleResolver" />
    <!-- 쿠키를 이용한 Locale 이용시 <bean id="localeResolver" class="org.springframework.web.servlet.i18n.CookieLocaleResolver"/> -->
    <bean id="localeChangeInterceptor" class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor">
        <property name="paramName" value="language" />
    </bean>
    
    
    
    
    <!-- 인터셉터 등록 -->
    <mvc:interceptors>
    	<bean class="net.dangdang.cmn.interceptor.AuthrtyChckInterceptor"/>
    	
    	<bean class="org.springframework.mobile.device.DeviceResolverHandlerInterceptor" />
	</mvc:interceptors>
    
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver" >
    	<property name="maxUploadSize" value="10485760"></property>
    	<property name="defaultEncoding" value="UTF-8"></property>
    </bean>


    <bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
        <property name="defaultErrorView" value="cmmn/egovError"/>
        <property name="exceptionMappings">
            <props>
                <prop key="org.springframework.dao.DataAccessException">cmmn/dataAccessFailure</prop>
                <prop key="org.springframework.transaction.TransactionException">cmmn/transactionFailure</prop>
                <prop key="egovframework.rte.fdl.cmmn.exception.EgovBizException">cmmn/egovError</prop>
                <prop key="org.springframework.security.AccessDeniedException">cmmn/egovError</prop>
            </props>
        </property>
    </bean>

    <bean class="org.springframework.web.servlet.view.UrlBasedViewResolver" p:order="1"
	    p:viewClass="org.springframework.web.servlet.view.JstlView"
	    p:prefix="/WEB-INF/jsp/" p:suffix=".jsp" p:contentType="text/html;charset=UTF-8" />

    <!-- For Pagination Tag -->
    <bean id="imageRenderer" class="net.dangdang.cmn.web.ImgPaginationRenderer"/>
    <bean id="imageRendererAdmin" class="net.dangdang.cmn.web.ImgPaginationAdminRenderer"/>

    <bean id="paginationManager" class="egovframework.rte.ptl.mvc.tags.ui.pagination.DefaultPaginationManager">
        <property name="rendererType">
            <map>
                <entry key="image" value-ref="imageRenderer"/>
                <entry key="image2" value-ref="imageRendererAdmin"/>
            </map>
        </property>
    </bean>
	<!-- /For Pagination Tag -->

    <mvc:view-controller path="/cmmn/validator.do" view-name="cmmn/validator"/>
    
    
    
    <!-- MULTIPART RESOLVERS -->
	<!-- regular spring resolver -->
	<bean id="spring.RegularCommonsMultipartResolver"
		class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<property name="maxUploadSize" value="100000000" />
		<property name="maxInMemorySize" value="100000000" />
	</bean>
	
	<alias name="spring.RegularCommonsMultipartResolver" alias="multipartResolver" />
	
	<!-- Json -->
	<bean id="jsonView" class="net.sf.json.spring.web.servlet.view.JsonView">
		<property name="contentType" value="application/json;charset=UTF-8" />
	</bean>
	
	<bean id="beanNameResolver" class="org.springframework.web.servlet.view.BeanNameViewResolver" p:order="0" />

	<!-- 엑셀 다운 -->
	<bean id="excelView" class="net.dangdang.cmn.web.EgovExcel" />
	
	<bean id="mailSender" class="org.springframework.mail.javamail.JavaMailSenderImpl">
			<property name="javaMailProperties">
				<props>
					<prop key="mail.smtp.starttls.enable">true</prop>
					<prop key="mail.smtp.auth">true</prop>
<!-- 					<prop key="mail.debug">true</prop> -->
				</props>
			</property>
			<property name="defaultEncoding" value="utf-8" />
			<property name="protocol" value="smtps" />
			
			<!-- mail server -->
			<property name="username" value="matchingchancebiz@gmail.com" />
			<property name="password" value="mcbiz15224422" />
			
			<property name="host" value="smtp.gmail.com" />
			<property name="port" value="465" />
		</bean>
	
	<mvc:default-servlet-handler/>
	
	<!-- transaction 패키지 설정-->
	<aop:config proxy-target-class="true">
		<aop:pointcut id="txAdvisePointCut" expression="execution(* kr.co.mcmall.mcm..Controller.*(..))"/>
		<aop:advisor id="transactionAdvisor" pointcut-ref="txAdvisePointCut" advice-ref="txAdvice"/>
	</aop:config>
    
    
    
</beans>
