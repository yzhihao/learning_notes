#Spring MVC_学习笔记1
## 基本概念
![](http://img.mukewang.com/5790f107000157b912800718.jpg)
[springmvc教程](http://www.cnblogs.com/sunniest/p/4555801.html) (包括设置一个自定义拦截器)

---
* view视图层：为用户提供UI，重点关注数据的呈现
model模型层：业务数据的信息表示，关注支撑业务的信息构成，通常是多个业务实体的组合
* contro
* MVC是一种架构模式，程序分层，分工合作，既相互独立，又协同工作


[Spring与SpringMVC的容器关系分析](http://www.imooc.com/article/6331)

##两个容器的理解
父容器是spring，就想全局变量，全局变量不可以应用局部变量
子容器是springmvc，就想局变量，局部变量可以应用全局变量

##json 传输
	@ResponseBody
	@RequestMapping("/testJson")
	public Collection<Employee> testJson(){
		return employeeDao.getAll();
	}
##自定义的converter
**从一串字符串到一个对象的转换**
####下面是自定义的文件
	@Component
	public class EmployeeConverter implements Converter<String, Employee> {
	@Override
	public Employee convert(String source) {
		if(source != null){
			String [] vals = source.split("-");
			//GG-gg@atguigu.com-0-105
			if(vals != null && vals.length == 4){
				String lastName = vals[0];
				String email = vals[1];
				Integer gender = Integer.parseInt(vals[2]);
				Department department = new Department();
				department.setId(Integer.parseInt(vals[3]));
				
				Employee employee = new Employee(null, lastName, email, gender, department);
				System.out.println(source + "--convert--" + employee);
				return employee;
			}
		}
		return null;
	}

###xml的配置
	<!--    <mvc:annotation-driven conversion-service="conversionService"></mvc:annotation-driven>	
		
	配置 ConversionService
	<bean id="conversionService"
		class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
		<property name="converters">
			<set>
				<ref bean="employeeConverter"/>
			</set>
		</property>	
	</bean> -->
###controller的写法
	@RequestMapping("/testConversionServiceConverer")
	public String testConverter(@RequestParam("employee") Employee employee){
		System.out.println("save: " + employee);
		employeeDao.save(employee);
		return "redirect:/emps";
	}
        