title: "如何减少magento后台语言界面的选项范围和数量"
id: 959
date: 2013-10-06 20:34:16
tags: 
categories: 
- magento
---

前面我们介绍了一片如何隐藏国家语言的文章，虽然可以完美屏蔽后台左下角的下拉框，但是因为我们修改的代码是注释掉多余的国家代码，导致在其他地方引用到国家的时候只剩下中国和美国两个，对于广大外贸的朋友肯定是不希望看到这样结局的。
<!-- more -->
那么我们有什么办法可以避免呢？有的。我们可以在后台显示页面的地方通过foreach循环保留只需要用到的后台语言，然后添加到左下角的下拉框即可。

修改的代码在
app/code/core/Mage/Adminhtml/Block/Page/Footer.php,
你可将这个代码拷贝到
/app/code/local/Mage/Adminhtml/Block/Page

没有的文件夹目录我们可以自己手动新建。

用文本编辑器打开footer文件，找到 getLanguageSelect()函数，我们需要修改这个函数哦。

修改后的代码如下：

[php]

public function getLanguageSelect()
 {
 $locale = Mage::app()-&gt;getLocale();
 $cacheId = self::LOCALE_CACHE_KEY . $locale-&gt;getLocaleCode();
 $html = Mage::app()-&gt;loadCache($cacheId);

$oldlocal = $locale-&gt;getTranslatedOptionLocales();

$newlocal = array();
 foreach ($oldlocal as $value) {
 if($value['value']=='en_GB' || $value['value']=='en_US' || $value['value']=='zh_CN' )
 $newlocal[]=$value;
 }

if (!$html) {
 $html = $this-&gt;getLayout()-&gt;createBlock('adminhtml/html_select')
 -&gt;setName('locale')
 -&gt;setId('interface_locale')
 -&gt;setTitle(Mage::helper('page')-&gt;__('Interface Language'))
 -&gt;setExtraParams('style=&quot;width:200px&quot;')
 -&gt;setValue($locale-&gt;getLocaleCode())
 // -&gt;setOptions($locale-&gt;getTranslatedOptionLocales())
 -&gt;setOptions($newlocal)
 -&gt;getHtml();
 Mage::app()-&gt;saveCache($html, $cacheId, array(self::LOCALE_CACHE_TAG), self::LOCALE_CACHE_LIFETIME);
 }

return $html;
 }

[/php]

减少magento后台语言界面的选项范围和数量,你操作就方便多了.