# TODO

## CI

目前采用JupyterBook临时上线到Web。未来考虑使用自己开发的Flutter库内置到APP中，以方便全平台访问。

目前上线版本，文档首页路由有异常。
`/docs/`重定向到`/docs/README.html`正常，`/docs`会重定向到`/README.html`。
猜测最可能的原因是JB框架的重定向策略。也有一定可能通过合理设置COS的重定向策略解决。
