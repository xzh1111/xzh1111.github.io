# Go template


### template

1. **动作(Action)**
   - 插值(Interpolation)
     ```go
     {{ . }}                  // 输出当前对象
     {{ .FieldName }}         // 访问当前对象的字段或方法
     ```
2. **管道(Pipelines)**
   - 可以将一个函数的输出作为另一个函数的输入
     ```go
     {{ .Title | toUpper | printf "%s is the title" }} // 首先将.Title转化为大写，然后格式输出
     ```
3. **控制结构(Control Structures)**
   - 条件语句
     ```go
     {{ if .IsTrue }}         // 如果.IsTrue为真，则执行这里的代码
     ...
     {{ else }}               // 否则，执行这里的代码
     ...
     {{ end }}
     ```
   - 循环语句
     ```go
     {{ range .Items }}       // 迭代.Items切片的每一项
     ...
     {{ end }}
     ```
4. **变量(Variables)**
   - 在模板中声明和使用局部变量
     ```go
     {{ $x := .Var }}         // 声明变量$x 并赋值为.Var
     ```
5. **模板引用(Template Reference)**
   - 在一个模板中调用另一个模板
     ```go
     {{ template "name" . }}  // 包含模板"name"
     ```
6. **注释(Comments)**
   - 在模板中添加注释
     ```go
     {{/* 这是一条注释 */}}    // A Comment
     ```


