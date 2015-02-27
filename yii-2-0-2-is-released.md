# Yii 2.0.2 ������ #

���Ǻܸ��˵����� Yii Framework 2.0.2 �汾�����ˡ���ο� http://www.yiiframework.com/download/ ���˵����װ���������ð汾��

2.0.2 �汾�� Yii 2.0 ������һ���������а���Լ40����Ҫ���¹��ܺ�bug�޸����仯�������б�����ڸ�����־���ҵ����ڴ�����Ҫ��л�������ʱ�������� Yii Ч�ʺ�ʹ���ܹ����������й����ߡ�

�������ѭ Yii 2 �Ŀ���������Ҫ�� Yii 2.0 GitHub ����Ŀ������Լ��� Yii Twitter ����߼��� Yii Facebook ������ϵ���� Yii �����ߡ�

���������ܽ���һЩ����Ҫ�����������ڱ�ƪ�����С�

## ·������

��ǰ�� ���Ŀ�ܵĴ���ֻ֧�ֱ�ʾ�ļ�·������ַ���������������˶�·��������֧�֡��ر��ǣ� ������Ϊ·������������Ȼ���ñ���������·��ʱ������ҪΪ������һ��URL��·��������Ҫͨ�������෽�� Url::to() �� Url::toRoute() ��֧�֡����磬

```php
use yii\helpers\Url;
 
Yii::setAlias('@posts', 'post/index');
 
// /index.php?r=post/index
echo Url::to(['@posts']);
echo Url::toRoute('@posts');
```

You may find route alias to be useful when your route design is not fixed and you want to avoid changing your URL creation code everywhere when your route design is changed.

## �����������

Many components contain properties that should be configured to be the ID of a dependent application component, ���� yii\caching\DbCache::db, yii\web\CacheSession::cache����ʱ��to avoid introducing new application component or for unit testing purpose, you may want to directly configure such a property with a configuration array that can be used to create the dependent component. You can do so now like the following:

```php
$cache = Yii::createObject([
    'class' => 'yii\caching\DbCache',
    'db' => [
        'class' => 'yii\db\Connection',
        'dsn' => '...',
    ],
]);
```

��������ڿ���һ���������ⲿ��������࣬ ������ʹ�����·��������׵õ����Ƶ�֧�֣�

```php
use yii\base\Object;
use yii\db\Connection;
use yii\di\Instance;
 
class MyClass extends Object 
{
    public $db = 'db';
 
    public function init() 
    {
        $this->db = Instance::ensure($this->db, Connection::className());
    }
}
```

����Ĵ��뽫���� db ���Ա�����Ϊ�����κγ�ʼֵ:

    һ���ַ����� ��ʾӦ�ó�������� ID��
    һ�� yii\db\Connection ʵ����
    һ�������������ڴ��� yii\db\Connection ʵ����


## Immutable Slug

�����ʹ�� yii\behaviors\SluggableBehavior�����������ʹ��һ���̶�����������һ�������ԡ�ͨ�����ø�����Ϊ true��if a slug has been generated before, �������ٱ��ı䣬��ʹ��Ӧ��Դ����ֵ�ı䡣 ��� SEO �������Ŀ���ô���Ϊ�㲻��ı�ʹ��slug�Ĺ����� URL��

## DatePicker Language Fallback

yii\jui\DatePicker widget ����֧������ fallback��This is useful when you configure its language property as a locale ID that contains region and/or variant part. ���磬if you set language to be de-DE, and the widget cannot find a language file named /ui/i18n/datepicker-de-DE.js, it will fall back to the language de and attempt the language file /ui/i18n/datepicker-de.js.

## ͨ����֤����

yii\base\Model ��������һ�����ķ��� addErrors() ���������һ��ģ��ת����֤������һ��ģ�͡����磬 �����һ������ ActiveRecord ģ�����һ����ģ���࣬����ͨ�� ActiveRecord ��ģ������֤���������ֻͨ�����ô˷�����

```php
use yii\base\Model;
use yii\db\ActiveRecord;
 
class MyForm extends Model 
{
    public $model;
 
    public function process()
    {
        // ...
        if (!$this->validate()) {
            $this->model->addErrors($this->getErrors());
            // ....
        }
    }
}
```