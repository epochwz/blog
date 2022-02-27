# 抽象Bean与子Bean
抽象Bean:提取多个<bean />配置中的相同信息,集中成配置模板,仅用于被继承,不能实例化,不需要class属性[若存在则子Bean可继承,即和父Bean使用相同的实现类]

子Bean可以从抽象Bean继承实现类、构造器参数、属性值等配置信息,且可以通过新增配置信息覆盖父Bean的配置信息。

子Bean无法从父Bean继承`depends-on、autowire、singleton、scope、lazy-init`属性,不指定时总是使用默认值

```xml
<bean id="personTemplete" abstract="true">
    <property name="name" value="纪伟忠" />
</bean>
<bean id="chinese" class="Chinese" parent="personTemplate" />
<bean id="american" class="American" parent="personTemplate" />
```

# 容器中的工厂Bean--实现FactoryBean接口
程序中通过getBean方法获取到的不是FactoryBean实现类的实例,而是FactoryBean的getObject()方法的返回值
1. 实现FactoryBean接口开发工厂Bean`GetFiledFactoryBean.java`
    ```java
    package org.jwz.factorybean;

    import org.springframework.beans.factory.FactoryBean;
    
    import java.lang.reflect.Field;
    
    public class GetFileldFactoryBean implements FactoryBean {
        private String targetClass;
        private String targetField;
    
        public void setTargetClass(String targetClass) {
            this.targetClass = targetClass;
        }
    
        public void setTargetField(String targetField) {
            this.targetField = targetField;
        }
    
        @Override
        // 返回工厂Bean生产的产品
        public Object getObject() throws Exception {
            Class<?> clazz = Class.forName(targetClass);
            Field field = clazz.getField(targetField);
            return field.get(null);
        }
    
        @Override
        // 获取工厂Bean所生产的实例的类型
        public Class<?> getObjectType() {
            return Object.class;
        }
    
        @Override
        // 返回该工厂Bean所生产的实例的产品是否为实例
        public boolean isSingleton() {
            return false;
        }
    }
    ```
2. 配置工厂Bean
    ```xml
    执行setter后额外多执行getObject方法,并将其返回值作为该Bean的实例
    <bean id="north" class="org.jwz.factorybean.GetFileldFactoryBean">
        <property name="targetClass" value="java.awt.BorderLayout"/>
        <property name="targetField" value="NORTH"/>
    </bean>
    ```
3. 获取FactoryBean的产品及其本身
    ```java
    // 获取FactoryBean所生产的产品
    System.out.println(ctx.getBean("north"));
    // 获取FactoryBean本身
    System.out.println(ctx.getBean("&north"));
    ```


Bean获取其所在的Spring容器--实现BeanFactoryAware接口
- 该接口只有一个方法:setBeanFactory(BeanFactory beanFactory)
- Spring容器会检测容器中的所有Bean,若发现某个Bean实现了BeanFactoryAware接口,则在创建该Bean实例之后将自身作为参数并立即调用setBeanFactory()方法

Bean获取其在容器中的id--实现BeanNameAware接口

depends-on属性显式指定被依赖Bean在目标Bean之前初始化
