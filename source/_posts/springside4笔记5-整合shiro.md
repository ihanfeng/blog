title: springside4笔记5-整合shiro
date: 2013-12-10 10:46:48
categories: SpringSide4
tags: [Spring,SpringSide4,shiro]
---
##**web.xml**

首先是在web.xml加入shiro配置。

代码：
{%codeblock lang:xml%}
<!-- 配置Shiro过滤器,先让Shiro过滤系统接收到的请求 -->  
<!-- 这里filter-name必须对应applicationContext.xml中定义的<bean id="shiroFilter"/> -->  
<!-- 使用[/*]匹配所有请求,保证所有的可控请求都经过Shiro的过滤 -->  
<!-- 通常会将此filter-mapping放置到最前面(即其他filter-mapping前面),以保证它是过滤器链中第一个起作用的 -->  
<filter>
	<filter-name>shiroFilter</filter-name>
	<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
	<init-param>
	  <!-- 该值缺省为false,表示生命周期由SpringApplicationContext管理,设置为true则表示由ServletContainer管理 -->  
		<param-name>targetFilterLifecycle</param-name>
		<param-value>true</param-value>
	</init-param>
</filter>
<filter-mapping>
	<filter-name>shiroFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
{%endcodeblock%}
<!-- more -->

##**applicationContext-shiro.xml**

该配置文件主要是对Shiro安全配置。
{%codeblock lang:xml%}
<!-- Shiro's main business-tier object for web-enabled applications -->
<bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
	<property name="realm" ref="shiroDbRealm" />
	<property name="cacheManager" ref="shiroEhcacheManager" />
</bean>
{%endcodeblock%}

继承自AuthorizingRealm的自定义Realm,即指定Shiro验证用户登录的类为自定义的ShiroDbRealm.java

{%codeblock lang:xml%}
<!-- 項目自定义的Realm, 所有accountService依赖的dao都需要用depends-on声明 -->
<bean id="shiroDbRealm" class="org.springside.examples.quickstart.service.account.ShiroDbRealm" depends-on="userDao,taskDao">
	<property name="accountService" ref="accountService"/>
</bean>
{%endcodeblock%}

用户授权信息Cache, 采用EhCache

{%codeblock lang:xml%}
<!-- 用户授权信息Cache, 采用EhCache -->
<bean id="shiroEhcacheManager" class="org.apache.shiro.cache.ehcache.EhCacheManager">
	<property name="cacheManagerConfigFile" value="classpath:ehcache/ehcache-shiro.xml"/>
</bean>
{%endcodeblock%}

配置Shiro过滤器

{%codeblock lang:xml%}
<!-- Shiro Filter -->
<bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
	<property name="securityManager" ref="securityManager" />
	<property name="loginUrl" value="/login" />
	<property name="successUrl" value="/" />
	<property name="filterChainDefinitions">
		<value>
			/login = authc
			/logout = logout
			/static/** = anon
			/api/** = anon
			/register/** = anon
			/admin/** = roles[admin]
			/** = user
		</value>
	</property>
</bean>
{%endcodeblock%}

{%codeblock lang:xml%}
<!-- 保证实现了Shiro内部lifecycle函数的bean执行 -->
<bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>
{%endcodeblock%}


