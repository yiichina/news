**Twig 扩展 2.0.5 版本发布了**

我们很高兴的宣布 Twig 扩展 2.0.5 版本发布了，其中包含3个小的新功能：

* 扩展简单的功能和简单的过滤器支持。
* 增加 @app/views, @app/modules, @app/widgets as Twig_Loader_Filesystem 加载器路径, 同样为主题 pathMap 路径。
* `register_asset_bundle()` 能够在第二个参数为真时返回 AssetBundle 实例。

关于这个版本更新的完整列表，请参阅 [CHANGELOG](https://github.com/yiisoft/yii2-twig/blob/2.0.5/CHANGELOG.md)。

注意，这可能是 2.0.x 的最后一版，下一个版本将是 2.1.x，这可能会破坏向后兼容性。如果你想停留在 2.0.x 版本，确保在你的 composer.json 中使用 ~2.0.0。