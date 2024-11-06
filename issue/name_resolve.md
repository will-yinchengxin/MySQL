## skip_name_resolve
线上碰到一下报错
```
[Warning] Ip address.'172.16.2.128' could not be resolved: Name or service not known
````
检查一下变量状态
```sql
mysql> show VARIABLES like "skip_name_resolve"
    -> ;
+-------------------+-------+
| Variable_name     | Value |
+-------------------+-------+
| skip_name_resolve | OFF    |
+-------------------+-------+
1 row in set (0.01 sec)
````

skip_name_resolve 是 MySQL 中的一个配置参数，其主要作用是控制 MySQL 服务器是否对客户端连接请求中的主机名进行 DNS 解析。具体来说，当这个参数被设置时，MySQL 服务器将不再尝试解析客户端的主机名，而是直接使用 IP 地址进行连接验证和处理
这样做的好处包括：

- 提高性能：禁用 DNS 解析可以减少连接时延，特别是在 DNS 查询较慢或不可靠的环境中，可以显著提高连接速度和查询性能
- 减少安全风险：通过避免域名解析，可以减少依赖于外部 DNS 服务的潜在安全风险
- 避免DNS问题：如果 DNS 服务器配置不正确或者客户端主机名无法解析，可能会导致连接失败或延迟。禁用 DNS 解析可以避免这些问题


然而，使用 skip_name_resolve 也有一些注意事项和限制：
- 无法使用主机名进行连接：一旦启用 skip_name_resolve，MySQL 将无法使用主机名进行连接，只能使用 IP 地址。这意味着在 MySQL 的授权表中也只能使用 IP 地址，而不能使用主机名
- 需要重新启动 MySQL 服务器：修改 skip_name_resolve 配置后，需要重新启动 MySQL 服务器才能使更改生效


将其设置为 ON 即可, 可以通过命令也可以在配置文件中配置

总的来说，skip_name_resolve 参数可以在提高 MySQL 服务器性能和安全性方面发挥作用，尤其是在面对慢速 DNS 或大量客户端连接的情况下。但是，它也限制了使用主机名进行连接和权限控制的能力，因此在决定是否启用此参数时需要权衡这些因素
