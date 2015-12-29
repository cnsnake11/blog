# todo list
1. **相对路径的图片，是否是从服务器下载的，如何做到下载一次就就不用下载了，脱离服务器**
	2. 应该是相对这个生成的js的某个文件夹会存，和js一起打包到app中即可，待验证
2. **高宽未知的图片的应用方案。**
1. **初步了解不可变数据框架**ok
2. 初步了解mixin
2. 将通用组件提交到npm中
1. 重构使用react提供的propTypes，而不是options，并更新文档;重构propsConcat，只校验，不赋值，使用...this.props的方式赋值。这样不好，一些没用的也会赋值上去，要找一个好的解决方案;
2. 使用RN组件规范的displayName代替自定义的_compName;
2. 搜索历史的存储和展现
5. listview的下拉刷新
6. detail页面的上拉查看详情
6. **RN的mvc开发框架flux**ok
7. RN动画与CSS3动画的异同
8. **RN中cookie的使用与测试**
9. filder的使用推广
10. fetch是否需要封装
11. 校验的封装
12. **第三方开发组件的集成规范和方案**
13. 组件间通信的封装
	14. http://www.ghugo.com/react-native-communicate/
	15. http://www.ghugo.com/react-native-event-emitter/
16. 面向状态设计还是面向接口设计的思考
	17. 尽量沿用RN思想，面向状态设计，参考modal组件
17. 文档采用代码即文档的模式
18. 因为css里没有选择器，给css起名字很痛苦，有没有好办法
19. **原生在调用一个rn页面时候，如何给rn页面传递参数？**
20. ReactNative的组件状态对渲染范围的影响.todo待测试
	21. //todo 思考：再封装一个按钮的组件，自己来存自己是否选中的状态；
	22. //todo 但是此组件如何与自己的兄弟组件进行交互呢？点击此此组件要消除到其它已选的组件。
	23. //兄弟组件间通信，肯定要借助父组件进行，在理解RN特性【state的改变只影响自己和自己的子组件的渲染】
	24. 【这个特性理解的好像不对，有空测试一下】的基础下，尽量只改变本应该改变的组件的state的状态值
1. 在基类BaseLogicObj的构造器中对对象的所有方法进行代理-todo待验证
2. gradle
3. graphql
4. 编码规范重构，参考：https://github.com/sunnylqm/react-native-coding-style
5. className react 的 插件

