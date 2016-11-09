我们很高兴的宣布 Imagine 扩展 2.0.4 版本发布了。

唯一的变化是 `Image::thumbnail()`  现在可以基于原始图像的宽高比自动计算缩略图尺寸。`Image::$thumbnailBackgroundColor` 和 `Image::$thumbnailBackgroundAlpha`  被引入以能够配置缩略图的填充颜色。

关于这个版本更新的完整列表，请参阅 [CHANGELOG](https://github.com/yiisoft/yii2-imagine/blob/2.0.4/CHANGELOG.md)。

注意，这可能是 2.0.x 的最后一版，下一个版本将是 2.1.x，这可能会破坏向后兼容性。如果你想停留在 2.0.x 版本，确保在你的 composer.json 中使用 ~2.0.0。
