# CSS

Cascading Style Sheet:层叠样式表语言

修饰HTML页面某些元素样式，让HTML页面更好看。

**在HTML页面嵌套CSS三种方式：**

- 第一种：在标签内部使用style属性来设置元素的css样式（内联定义方式）

  ```html
  <标签 style="样式名:样式值;样式名:样式值;样式名:样式值;"></标签>
  ```

- 第二种：在head标签中使用 style块，这种方式被称为样式块方块

  ```html
  <head>
      <style type="text/css">
          选择器{
              样式名:样式值;
              样式名:样式值;
  		}
          选择器{
              样式名:样式值;
              样式名:样式值;
  		}
  </head>
  ```

- 第三种：链入外部样式表文件，最常用

  ```html
  <link type="text/css" rel="stylesheet" href="css文件路径">
  ```

  **这种方式易维护，维护成本较低**