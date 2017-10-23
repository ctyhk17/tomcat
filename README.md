# tomcat
my study
<?xml version="1.0" encoding="UTF-8"?>

-<beans xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd" xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:aop="http://www.springframework.org/schema/aop" xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.springframework.org/schema/beans">


-<context:component-scan base-package="egovframework,net,kr">

<context:include-filter expression="org.springframework.stereotype.Controller" type="annotation"/>

<context:exclude-filter expression="org.springframework.stereotype.Service" type="annotation"/>

<context:exclude-filter expression="org.springframework.stereotype.Repository" type="annotation"/>

</context:component-scan>


-<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">


-<property name="webBindingInitializer">

<bean class="net.dangdang.cmn.web.EgovBindingInitializer"/>

</property>

</bean>


-<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping">


-<property name="interceptors">


-<list>

<ref bean="localeChangeInterceptor"/>

</list>

</property>

</bean>

<bean class="org.springframework.web.servlet.i18n.SessionLocaleResolver" id="localeResolver"/>

<!-- 쿠키를 이용한 Locale 이용시 <bean id="localeResolver" class="org.springframework.web.servlet.i18n.CookieLocaleResolver"/> -->



-<bean class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor" id="localeChangeInterceptor">

<property name="paramName" value="language"/>

</bean>

<!-- 인터셉터 등록 -->



-<mvc:interceptors>

<bean class="net.dangdang.cmn.interceptor.AuthrtyChckInterceptor"/>

<bean class="org.springframework.mobile.device.DeviceResolverHandlerInterceptor"/>

</mvc:interceptors>


-<bean class="org.springframework.web.multipart.commons.CommonsMultipartResolver" id="multipartResolver">

<property name="maxUploadSize" value="10485760"/>

<property name="defaultEncoding" value="UTF-8"/>

</bean>


-<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">

<property name="defaultErrorView" value="cmmn/egovError"/>


-<property name="exceptionMappings">


-<props>

<prop key="org.springframework.dao.DataAccessException">cmmn/dataAccessFailure</prop>

<prop key="org.springframework.transaction.TransactionException">cmmn/transactionFailure</prop>

<prop key="egovframework.rte.fdl.cmmn.exception.EgovBizException">cmmn/egovError</prop>

<prop key="org.springframework.security.AccessDeniedException">cmmn/egovError</prop>

</props>

</property>

</bean>

<bean class="org.springframework.web.servlet.view.UrlBasedViewResolver" p:contentType="text/html;charset=UTF-8" p:suffix=".jsp" p:prefix="/WEB-INF/jsp/" p:viewClass="org.springframework.web.servlet.view.JstlView" p:order="1"/>

<!-- For Pagination Tag -->


<bean class="net.dangdang.cmn.web.ImgPaginationRenderer" id="imageRenderer"/>

<bean class="net.dangdang.cmn.web.ImgPaginationAdminRenderer" id="imageRendererAdmin"/>


-<bean class="egovframework.rte.ptl.mvc.tags.ui.pagination.DefaultPaginationManager" id="paginationManager">


-<property name="rendererType">


-<map>

<entry key="image" value-ref="imageRenderer"/>

<entry key="image2" value-ref="imageRendererAdmin"/>

</map>

</property>

</bean>

<!-- /For Pagination Tag -->


<mvc:view-controller view-name="cmmn/validator" path="/cmmn/validator.do"/>

<!-- MULTIPART RESOLVERS -->


<!-- regular spring resolver -->



-<bean class="org.springframework.web.multipart.commons.CommonsMultipartResolver" id="spring.RegularCommonsMultipartResolver">

<property name="maxUploadSize" value="100000000"/>

<property name="maxInMemorySize" value="100000000"/>

</bean>

<alias name="spring.RegularCommonsMultipartResolver" alias="multipartResolver"/>

<!-- Json -->



-<bean class="net.sf.json.spring.web.servlet.view.JsonView" id="jsonView">

<property name="contentType" value="application/json;charset=UTF-8"/>

</bean>

<bean class="org.springframework.web.servlet.view.BeanNameViewResolver" id="beanNameResolver" p:order="0"/>

<!-- 엑셀 다운 -->


<bean class="net.dangdang.cmn.web.EgovExcel" id="excelView"/>


-<bean class="org.springframework.mail.javamail.JavaMailSenderImpl" id="mailSender">


-<property name="javaMailProperties">


-<props>

<prop key="mail.smtp.starttls.enable">true</prop>

<prop key="mail.smtp.auth">true</prop>

<!-- <prop key="mail.debug">true</prop> -->


</props>

</property>

<property name="defaultEncoding" value="utf-8"/>

<property name="protocol" value="smtps"/>

<!-- mail server -->


<property name="username" value="matchingchancebiz@gmail.com"/>

<property name="password" value="mcbiz15224422"/>

<property name="host" value="smtp.gmail.com"/>

<property name="port" value="465"/>

</bean>

<mvc:default-servlet-handler/>

<!-- transaction 패키지 설정-->



-<aop:config proxy-target-class="true">

<aop:pointcut expression="execution(* kr.co.mcmall.mcm..Controller.*(..))" id="txAdvisePointCut"/>

<aop:advisor id="transactionAdvisor" advice-ref="txAdvice" pointcut-ref="txAdvisePointCut"/>

</aop:config>

</beans>
