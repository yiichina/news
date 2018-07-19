我们很高兴的宣布 Gii 扩展 2.0.6 版本发布了。此版本修复了很多 bug 当然也加了些新功能：

# 概况

* 兼容 PHP 7.1 
* 现在可以在生成器模板中使用别名。
* 在生成的文件列表预览中添加了文件名的过滤。
* 添加了 meta 标签来阻止当搜索引擎时暴露调试 debug 的出现。

# CRUD 生成器

关于代码生成的变化：

* 现在 @throws 标签抛出了 404 个异常。
* NotFoundHttpException 消息使用 i18n。
* 删除后返回冗余。
* 从注释的代码中删除空格，因此 IDE 的注释中无空格
* 提交按钮标签从 "Update" 和 "Create" 改为 "Save"。
* 改变了 CRUD 生成器翻译 "Update X id" 的方式。由于翻译困难，现在是一个完整的字符串。

# Model 生成器

"Use Table Prefix" 和 "Generate ActiveQuery" 选项现在也很棘手。此外，生成器也改善了：

* PostgreSQL got default validator with null value for integers in the model and ilike operator in search model.
* int 和 bool 被用来代替整数和布尔值。
* FK detection can now recognize id_something additionally to existing something_id.
* There's an option now to generate relations only for current schema. It's useful for Oracle and MSSQL where reading schema is slow.
* 表注释现在可以用于 PHPDoc 属性描述。
* 现在为具有多个主键的表生成唯一的验证规则。
* 参阅 CHANGELOG 查看详细信息。

感谢参与本次发布的所有贡献者。
