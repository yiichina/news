Yii 2.0.3 ������

���Ǻܸ��˵����� Yii Framework version 2.0.3 �汾�����ˡ���ο�˵�� http://www.yiiframework.com/download/ ��װ���������˰汾��

�汾 2.0.3 �� Yii 2.0 ��һ�������汾�����а����˴�Լ 50 ��С���¹��ܺ� bug �޸������µ������б������ [������־]���ҵ����ڴ�����Ҫ��л�������ʱ�������� Yii Ч�ʺ�ʹ���ܹ����������й����ߡ�

��Ҫ��л����ĵ������ͽ��ĵ�����ɶ�����ԵĹ����ߡ�

��������ѭ Yii 2 �Ŀ���������Ҫ�� Yii 2.0 GitHub ����Ŀ�������Լ��� Yii Twitter ����߼��� Yii Facebook ������ϵ���� Yii �����ߡ�

���������ܽ���һЩ����Ҫ�����������ڱ�ƪ�����С�

## ���ܿ�仯

�����ڲ���Ҫ��һ���Ƚϴ�ı仯�������Ѿ��� yii\base\Security ���� OpenSSL ȡ���� Mcrypt�����������Ѿ�8��û�и������ˣ�����л Tom Worster �ڱ��ָ߶Ȱ�ȫ�ͱ�׼�� Yii �������ĳ�ɫ�Ĺ��������� OpenSSL Ĭ��������ڽ��� PHP �У�������Ӧ�ò��������κ������ݵ����⡣�������������������������ǡ�

## RBAC ����

���������ʹ�����ݿ����洢 RBAC ���ݣ���ᷢ������ִ�в������룬��Ϊÿ�����ʼ�齫�漰��� SQL ����ִ�С�Ϊ��������ܣ������������ʵʩ���� yii\rbac\DbManager�����洢�ڻ����е����� RBAC ��νṹ�дӶ��������� checkAccess() �����ܡ�Ĭ������£�RBAC ����δ���á�������ͨ����Ӧ�ó������������� yii\rbac\DbManager�����£�

```php
return [
    'components' => [
        'authManager' => [
            'class' => 'yii\rbac\DbManager',
            'cache' => 'cache',   // this enables RBAC caching
        ],
        'cache' => [
            'class' => 'yii\caching\ApcCache',
        ]
        // ...
    ],
]
```

## ҳ�滺��

��ǰ��ҳ�滺����������� HTML ���ݻ����С��������ͼ��������Ӧ���� RESTful ���㽫�ᷢ������������Ӧ��ʽ����ȷ���������������ڴ˰汾�е���ߣ����������ʹ�� yii\filters\PageCache �����治ͬ�������Ӧ�����Լ���Ӧͷ�������磬��һ�� RESTful ���Ŀ��������У�����԰����·�ʽ��������������

```php
public function behaviors()
{
    return [
        [
            'class' => 'yii\filters\PageCache',
            'only' => ['index'],
            'duration' => 60,
        ],
    ];
}
```

## Cache Busting for Assets

��һ����ǿ�͹�������֧�� cache busting of published assets�����������������������е������� JS ���� CSS �ļ��� HTTP ����ʱ��ȱ���������������Щ�ļ������� HTTP ����ͻ�����Ȼʹ���ϰ汾������������ͨ������ yii\web\AssetManager::appendTimestamp ����������� JS �� CSS �ļ����档ͨ�����ø�����Ϊ true����Ӧ�� JS �� CSS �ļ����ļ��޸�ʱ�佫����ӵ�ÿ�� JS/CSS ��ַ�С���ˣ�ÿ���ļ����޸ģ��ͻ��˽�ʼ�ս��յ����°汾��

```
return [
    'components' => [
        'assetManager' => [
            'class' => 'yii\web\AssetManager',
            'appendTimestamp' => true,
        ],
        // ...
    ],
]
```

## �޸ĵ�ǰ�� URL

һ���µ������෽�� yii\helpers\Url::current() �ļ��������ͨ����ӻ�ɾ��һЩ GET ���������ɵ��޸ĵ�ǰ����� URL�����磬

```php
// assume $_GET = ['id' => 123, 'src' => 'google'], current route is "post/view"
 
// /index.php?r=post/view&id=123&src=google
echo Url::current();
 
// /index.php?r=post/view&id=123
echo Url::current(['src' => null]);
// /index.php?r=post/view&id=100&src=google
echo Url::current(['id' => 100]);
```

## ������־��ת

�����ʹ�� yii\log\FileTarget ������־�ļ�������������ͨ����������ĵ�һ����������������־�Զ���ת������ʹ���ⲿ������ת��־�ļ�ʱ���������õġ�

```php
return [
    'components' => [
        'log' => [
            'targets' => [
                [
                    'class' => 'yii\log\FileTarget',
                    'enableRotation' => false,
                ],
            ],
        ],
    ],
];
```

## ��������

����ʹ�� yii\helpers\Html ������ HTML ��ǩ���������Խ������⴦�����磬

```php
// displays: <div data-name="xyz" data-age="20"></div>
echo Html::tag('div', '', ['data' => ['name' => 'xyz', 'age' => 20]]);
```

���� ng ���Ժ� data-ng ����ͨ���˷���Ҳ�����⴦����������ʹ�� AngularJS ʱ���������õġ������������������ԣ����齫��תΪ JSON ��ʽ����Ƕ�뵽 HTML ��ǩ��ʱ��

�����ͨ���޸� yii\helpers\Html::$dataAttributes ������������һ������Ӧ����������ʾ��һ�����ر���

## Trimming Input

�����ʹ�õ��� trim ��֤�����㽫���� trimming Ҳ�����ڿͻ�����ɡ�Ҳ����˵���һ�� text input ��һ�� trim ��֤�����������������������ߵĿո���input�ؼ�ʧȥ�����ʱ��ᱻ�Զ��Ĺ��˵�������㲻ϲ����һ����Ϊ�������Խ� trim ��֤����� enableClientValidation ѡ������Ϊ false��

## �������󳤶�

��ʹ�� yii\helpers\Html::activeTextInput() ���� yii\widgets\ActiveField::textInput() ������һ���ļ����룬�����Ҫָ�����ɵ��ı������ maxlength ���ԡ�����㽫 string ��֤�������Ӧ�� model ����������������ϣ��������ʽָ����󳤶ȵ�ֵ����Ϊ������Ѿ����ַ���������ָ����������ͨ��������󳤶�ʵ����һĿ�꣬�����Զ�����ַ�����������ѡ���ֵ�����磬

```php
// assume "name" has a validation rule: ['name', 'string', 'max' => 128]
// generates: <input type="text" ... maxlength="128">
echo Html::activeTextInput($model, 'name', ['maxlength' => true]);
```

## ���ö���

һ���½ӿ���Ϊ yii\base\Configurable ���ڿ���Ϊ��������һ���ࡣ���һ����ʵ��������ӿڣ�yii\di\Container ����ʹ���캯�������һ����������һ���������顣���磬

```php
class Foo implements \yii\base\Configurable
{
    public function __construct($a, $b, $config = [])
    {
    }
}
 
$container = new \yii\di\Container;
$object = $container->get('Foo', [1, 2], ['prop1' => 3]);
// equivalent to: $object = new Foo(1, 2, ['prop1' => 3]);
```

�ڴ˰汾֮ǰ��Ϊ��ʵ����һ�����㽫���ò��� yii\base\Object ����չ���졣