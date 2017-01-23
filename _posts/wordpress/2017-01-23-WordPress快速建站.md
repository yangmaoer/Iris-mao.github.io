---
layout: post
title:  "WordPress快速建站"
date:   2017-01-12 10:19:19 +0800
categories: jekyll HTML5 wordpress
tags: wordpress
---

####WordPress快速建站

#####简单介绍：
<p>WordPress是一种使用PHP语言开发的博客平台，用户可以在支持PHP和MySQL数据库的服务器上架设属于自己的网站。也可以把 WordPress当作一个内容管理系统（CMS）来使用。
WordPress是一款个人博客系统，并逐步演化成一款内容管理系统软件，它是使用PHP语言和MySQL数据库开发的。用户可以在支持 PHP 和 MySQL数据库的服务器上使用自己的博客。
WordPress有许多第三方开发的免费模板，安装方式简单易用。不过要做一个自己的模板，则需要你有一定的专业知识。比如你至少要懂的标准通用标记语言下的一个应用HTML代码、CSS、PHP等相关知识。</p>

#####步骤：
* 下载wordpresscms安装包https://cn.wordpress.org/（官网）
* 将压缩包解压到配置环境中，我本地用的phpstudy配置环境，要配置php+mysql+apche环境，运行/wp-admin/setup-config.php
* 在本地数据库中新建wordpress数据库
* 在网页上运行/wp-admin/setup-config.php之后，点击按钮进行一步步安装，安装完成之后就进入了wordpress的后台界面，
![QQ截图20170123090721.png](C:\Users\Iris\Desktop\wprdpress\QQ截图20170123090721.png)
* 可以先在本地编写好你的静态页面之后，然后用wordpress进行套站
* 在/wp-content/themes文件夹下建立自己的主题文件夹，然后进入后台的《外观》-《主题》界面选择自己建的主题即可
![QQ截图20170123093432.png](C:\Users\Iris\Desktop\wprdpress\QQ截图20170123093432.png)
<p>1、如何把静态页面制作成主题
2、有多个CSS文件的时候，用哪个文件作为style.css
制作一个最简单的主题，只需要两个文件，index.php和style.css（放在当前文件夹下面）
第一步，准备静态页面
第二步，制作index.php和style.css
第三步，给style.css添加版权信息
第四步：把主题上传到空间中wordpress安装路径，wp-content/themes/下面，这里主题的文件夹名字必须是英文
第五步，在wordpress后台启用主题
先给style.css添加版权信息
   ```
    Theme Name: 自定义 
    Theme URI: http://www.***.com 
    Description: 自定义
    Author: 自定义 
    Author URI: http://www.***.com
    Version: 1.0 
    Tags: ***
    ```
Style.css路径调用：<?php bloginfo( 'stylesheet_url' ); ?>
主题文件夹路径：<?php bloginfo('template_directory'); ?>
</p>
* 制作顶部header.php和底部footer.php
需要用到的调用标签：
```
<?php get_header();?>
<?php get_footer();?>
<?php get_sidebar();?>
```
获取主页路径：```<?php echo get_option('home'); ?>```
Header.php中用到的标签：
```
<meta http-equiv="Content-Type" content="text/html; charset=<?php bloginfo( 'charset' ); ?>" />
<title><?php wp_title(''); ?><?php if(wp_title('', false)) { echo ' | '; } ?> <?php bloginfo('name'); ?></title>	
<?php wp_head(); ?>
```
自定义的导航调用方法：(分类栏目中的项目)
```
  <?php
$args=array(
  'orderby' => 'id',
  'order' => 'ASC'
  );
$categories=get_categories($args);
  foreach($categories as $category) { 
    echo '<li class="thisclass"><a href="' . get_category_link( $category->term_id ) . '" title="' . sprintf( __( "View all posts in %s" ), $category->name ) . '" ' . '>' . $category->name.'</a></li>';
    } 
?>
```
* 调用分类目录下的内容(array数组里面的数字就是对应的分类栏目的id)
```
<?php $display_categories = array(1,3,4,130,6,7,8,5); 
		foreach ($display_categories as $category) { ?>
<div class="P_category">
<?php query_posts("showposts=8&cat=$category")?>
<h2 class="P_c_one"><a href="<?php echo get_category_link($category);?>"><?php single_cat_title(); ?></a></h2>
    <ul class="p_news">
<?php while (have_posts()) : the_post(); ?>
    <li>· <a href="<?php the_permalink() ?>" title="<?php the_title(); ?>"><?php echo mb_strimwidth(get_the_title(), 0, 40, '…'); ?>
</a> </li>
<?php endwhile; ?>
    </ul>
    </div>
<?php } wp_reset_query();?>
```
* 每个分类栏目对应的页面是category.php，每个内容详情页对应的页面是single.php
* 在主题目录下新建functions.php，在里面写入以下方法，在后台《外观》菜单下面就会出现《菜单》选项，在后台就可以进行自定义菜单操作了
  ```
  if (function_exists('register_nav_menus')) {
    register_nav_menus(array(
        'primary' => '导航菜单',
    ));
}
  ```
  在header定义菜单的部分加上即可
   ```
   <?php
        if (function_exists('wp_nav_menu')) {
            wp_nav_menu(array(
                'theme_location' => 'primary',
                'menu_id' => 'nav',
                'container' => 'ul',
            ));
        }
        ?>
   ```
![QQ截图20170123100720.png](C:\Users\Iris\Desktop\wprdpress\QQ截图20170123100720.png)
* 经过以上步骤基本就能搭建一个简单的cms页面了，更多的可以到https://www.wpdaxue.com/进行学习
