# IDEA添加自定义注释

![image-20210227224544833](https://gitee.com/feigeCode/picture/raw/master/img/image-20210227224544833.png)





![image-20210227224650814](https://gitee.com/feigeCode/picture/raw/master/img/image-20210227224650814.png)



cc为类注释(使用cc)

~~~java
/**
 * @description:
 * @author: $author$
 * @date: $date$ $time$
 */
~~~

![image-20210227224911892](https://gitee.com/feigeCode/picture/raw/master/img/image-20210227224911892.png)



mc为类注释（使用/mc+回车键）

~~~java
**
 * @description: 
 * @author: $author$
 * @date: $date$ $time$
 $params$
 * @return: $return$
 */
~~~

![image-20210227224948629](https://gitee.com/feigeCode/picture/raw/master/img/image-20210227224948629.png)

脚本

~~~java
groovyScript("def result=''; def params=\"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); for(i = 0; i < params.size(); i++) {result+=' * @param\\t' + params[i] + '\\t' + ((i < params.size() - 1) ? '\\n' : '')}; return result", methodParameters())
~~~

