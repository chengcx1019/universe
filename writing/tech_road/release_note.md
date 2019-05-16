> 用于追踪新版本更新的变更，进行必要解读，重要的不是记录，而是能够心中有数。
>
> 在第一章节文档汇总记录所有的文档

比如python 3.6 to python 3.7

## 文档汇总

[paramiko](http://docs.paramiko.org/en/2.4/api/client.html)

## python更新

## Django新特性更新

> 关注django版本的更新，持续更细

### note 2.0

[文档](https://docs.djangoproject.com/en/2.1/releases/2.0/)

- 简化url路由语法

  添加`django.urls.path`,更简单和更易读的url路由语法

  from:

  `url(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),`

  to:

  `path('articles/<int:year>/', views.year_archive),`，支持强制类型的url参数

   `django.conf.urls.url`转变为 [`django.urls.re_path`](https://docs.djangoproject.com/en/2.1/ref/urls/#django.urls.re_path)

  

## jupyterlab

[JupyterLab Changelog](https://jupyterlab.readthedocs.io/en/stable/getting_started/changelog.html#changelog)

### jupyter extension

```
pip install jupyter_contrib_nbextensions && jupyter contrib nbextension install 
```

https://towardsdatascience.com/jupyter-notebook-extensions-517fa69d2231