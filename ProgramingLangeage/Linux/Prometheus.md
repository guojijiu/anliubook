### Prometheus

### 1. 软件安装
```
服务端程序，Prometheus版本：2.30.3.linux-amd64
wget https://github.com/prometheus/prometheus/releases/download/v2.29.2/prometheus-2.29.2.linux-amd64.tar.gz
被控端程序，用于采集监控指标数据，node_exporter版本：1.2.2.linux-amd64
wget https://github.com/prometheus/node_exporter/releases/download/v1.2.2/node_exporter-1.2.2.linux-amd64.tar.gz
用于配置故障告警，alertmanager版本：0.23.0.linux-amd64
wget https://github.com/prometheus/alertmanager/releases/download/v0.23.0/alertmanager-0.23.0.linux-amd64.tar.gz
用于图形界面展示，grafana版本：8.5.1.linux-amd64
wget https://dl.grafana.com/enterprise/release/grafana-enterprise-8.5.1.linux-amd64.tar.gz
```

### 2. 启动服务
```
1). 解压node_exporter-1.2.2.linux-amd64.tar.gz，重命名node_exporter，进入node_exporter，执行./node_exporter，浏览器输入http://ip:9100/，可以查看内容；
2). 解压prometheus-2.29.2.linux-amd64.tar.gz，重命名prometheus，进入prometheus（具体配置参考下面prometheus.yml），执行./prometheus，浏览器输入http://ip:9090/，在Status->Targets，可以查看内容；
3). 解压grafana-enterprise-8.5.1.linux-amd64.tar.gz，重命名grafana，进入grafana，执行./bin/grafana-server，浏览器输入http://ip:3000/（具体操作配置参考下面grafana）；
4). 解压alertmanager-0.23.0.linux-amd64.tar.gz，重命名alertmanager，进入alertmanager（具体配置参考下面alertmanager.yml），执行./alertmanager，浏览器输入http://ip:9093/，可以查看内容；
另：
使用：ss -ntlp | grep node_exporter/grafana-server/prometheus/alertmanager，查看服务启动情况
```

### 3. prometheus.yml
```
# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
           - localhost:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
   - "first_rules.yml"
  # - "second_rules.yml"

scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "my server" #对象名称，名称自定义
    static_configs:
      - targets: ["localhost:9100"] #指定要抓取的目标主机IP地址与端口

  - job_name: "docker" #对象名称，名称自定义
    static_configs:
      - targets: ["localhost:8088"] #指定要抓取的目标主机IP地址与端口
  - job_name: "node01" #对象名称，名称自定义
    static_configs:
      - targets: ["localhost:9100"] #指定要抓取的目标主机IP地址与端口
```

### 4. first_rules.yml
```
在prometheus目录下创建first_rules.yml预警规则（可参考https://awesome-prometheus-alerts.grep.to/，有各类配置规则），内容如下：

groups: #告警分组
- name: 'linux_status' #告警组名
  rules:
  - alert: "主机状态告警" #告警状态
    expr: up == 0 #告警指标，UP指标的值等于0表示Down状态，1表示正常
    for: 20s #等待20秒发送告警信息
    labels: #指定告警级别
      team: "node" #子标签（可与route下的配置group_by: ['alertname']匹配使用，一次来具体分类告警组，以不同team发送不同邮件）
      severity: '严重' #告警级别
    annotations: #告警信息描述
      summary: "主机为:【{{$labels.instance }}】出现：主机状态告警 " #描述信息自定义
      description: "主机【{{ $labels.instance }}】已经宕机20秒了！" #描述信息自定义
      summarys: "主机为:【{{$labels.instance }}】出现的故障已经处理完成！"
      value: "{{ $value }}" #定义使用本地时间（这也就是为什么要做同步时间的操作）

  - alert: "CPU利用率告警" #告警状态
    expr: sum by(instance) (avg without(cpu) (irate(node_cpu_seconds_total{mode!="idle"}[5m]))) > 0.6 #告警指标，cpu平均利用率超过60%
    for: 20s #等待20秒发送告警信息
    labels: #指定告警级别
      team: "node"
      severity: '告警' #告警级别
    annotations: #告警信息描述
      summary: "主机为:【{{$labels.instance }}】出现：CPU利用率达到60% "
      description: "主机【{{ $labels.instance }}】的CPU利用率已经达到60%了！"
      summarys: "主机为:【{{$labels.instance }}】出现的告警已有处理方案！"
      value: "{{ $value }}"

  - alert: "CPU利用率告警" #告警状态
    expr: sum by(instance) (avg without(cpu) (irate(node_cpu_seconds_total{mode!="idle"}[5m]))) > 0.85 #告警指标，cpu平均利用率超过85%
    for: 20s #等待20秒发送告警信息
    labels: #指定告警级别
      team: "node"
      severity: '严重' #告警级别
    annotations: #告警信息描述
      summary: "主机为:【{{$labels.instance }}】出现：CPU利用率达到85% "
      description: "主机【{{ $labels.instance }}】的CPU利用率已经达到85%了！"
      summarys: "主机为:【{{$labels.instance }}】出现的告警已有处理方案！"
      value: "{{ $value }}"

  - alert: "内存利用率告警" #告警状态
    expr: avg by(instance) ((1 - (node_memory_MemFree_bytes + node_memory_Cached_bytes + node_memory_Buffers_bytes) / node_memory_MemTotal_bytes ) * 100) > 70 #告警指标，内存平均利用率超过70%
    for: 3m #等待20秒发送告警信息
    labels: #指定告警级别
      team: "node"
      severity: '告警' #告警级别
    annotations: #告警信息描述
      summary: "主机为:【{{$labels.instance }}】出现：内存利用率达到70% "
      description: "主机【{{ $labels.instance }}】的内存利用率已经达到70%了！"
      summarys: "主机为:【{{$labels.instance }}】出现的告警已有处理方案！"
      value: "{{ $value }}"

  - alert: "内存利用率告警" #告警状态
    expr: avg by(instance) ((1 - (node_memory_MemFree_bytes + node_memory_Cached_bytes + node_memory_Buffers_bytes) / node_memory_MemTotal_bytes ) * 100) > 90 #告警指标，内存利用率超过90%
    for: 3m #等待20秒发送告警信息
    labels: #指定告警级别
      team: "node"
      severity: '严重' #告警级别
    annotations: #告警信息描述
      summary: "主机为:【{{$labels.instance }}】出现：内存利用率达到90% "
      description: "主机【{{ $labels.instance }}】的内存使用率已经达到90%了！"
      summarys: "主机为:【{{$labels.instance }}】出现的告警已有处理方案！"
      value: "{{ $value }}"

  - alert: "磁盘利用率告警" #告警状态
    expr: (1 - node_filesystem_free_bytes{fstype!="rootfs",mountpoint!="",mountpoint!~"/(run|var|sys|dev).*"} / node_filesystem_size_bytes) * 100 > 80 #告警指标，磁盘利用率超过80%
    for: 2m #等待20秒发送告警信息
    labels: #指定告警级别
      team: "node"
      severity: '告警' #告警级别
    annotations: #告警信息描述
      summary: "主机为:【{{$labels.instance }}】出现：磁盘利用率达到80% "
      description: "主机【{{ $labels.instance }}】的磁盘利用率已经达到80%了！"
      summarys: "主机为:【{{$labels.instance }}】出现的告警已有处理方案！"
      value: "{{ $value }}"

  - alert: "磁盘利用率告警" #告警状态
    expr: (1 - node_filesystem_free_bytes{fstype!="rootfs",mountpoint!="",mountpoint!~"/(run|var|sys|dev).*"} / node_filesystem_size_bytes) * 100 > 90 #告警指标，磁盘利用率超过90%
    for: 2m #等待20秒发送告警信息
    labels: #指定告警级别
      team: "node"
      severity: '严重' #告警级别
    annotations: #告警信息描述
      summary: "主机为:【{{$labels.instance }}】出现：磁盘利用率达到90% "
      description: "主机【{{ $labels.instance }}】的磁盘使用率已经达到90%了！"
      summarys: "主机为:【{{$labels.instance }}】出现的告警已有处理方案！"
      value: "{{ $value }}"
```

### 5. alertmanager.yml
```
# 全局配置
global:
  # 处理告警j时间
  resolve_timeout: 5m
  # mtp服务器
  smtp_smarthost: 'smtp.qq.com:465'
  # 发件人邮箱
  smtp_from: 'aaa@qq.com'
  # 发件人邮箱登录名
  smtp_auth_username: 'aaa@qq.com'
  # 这里是邮箱的授权密码，不是邮箱登录密码
  smtp_auth_password: '123455'
  # 不设置的话默认为 true，当为 true 时会有 starttls 错误，为了简单这里设置为 false
  smtp_require_tls: false
  smtp_hello: 'qq.com'

  # 指定告警模板文件位置
templates:
  # 模板文件地址，默认不存在，需要手动创建
  - '/usr/local/monitor/alertmanager/template/alert.tmpl'

route:
  # 分组标签，team为子标签
  group_by: ['alertname','team']
  # 告警等待时间。告警产生后等待10s，如果有同组告警一起发出
  group_wait: 10s
  # 两组告警的间隔时间
  group_interval: 10s
  # 重复告警的间隔时间，减少相同告警的发送频率 此处为测试设置为1分钟
  repeat_interval: 5m
  # 默认接收者
  receiver: 'email'

# 告警接收器
receivers:
# 接受名
- name: 'email'
  email_configs:
  # 收件人邮箱
  - to: 'aaa@qq.com'
  # 添加,指定template中的alert.html页面（与模板中的define对应）
    html: '{{ template "alert.html" . }}'
    send_resolved: true
    # 邮件标题
    headers: { Subject: "[重要] 来自监控系统的报警邮件 " }

# 告警抑制的配置
inhibit_rules:
- source_match:
    severity: '告警'
  target_match:
    severity: '严重'
  equal: ['alertname', 'dev', 'instance']
```

### 6. alert.tmpl
```
在alertmanager下创建template目录，添加alert.tmpl文件，内容如下：
{{ define "alert.html" }}
{{- if gt (len .Alerts.Firing) 0 -}}
{{- range $index, $alert := .Alerts -}}
========= 故障告警 ==========<br>
告警程序: {{ .Labels.job }} <br>
告警名称：{{ .Labels.alertname }}<br>
告警级别：{{ .Labels.severity }}<br>
告警机器：{{ .Labels.instance }} {{ .Labels.device }}<br>
告警详情：{{ .Annotations.summary }}<br>
告警时间：{{ (.StartsAt.Add 28800e9).Format "2006-01-02 15:04:05" }}<br>
#####此处"28800e9"指时间格式为CST（即北京时间），"2006-01-02 15:04:05"为时间格式，并不指所写时间。
========= END ==========<br>
{{- end }}
{{- end }}
{{- if gt (len .Alerts.Resolved) 0 -}}
{{- range $index, $alert := .Alerts -}}
========= 恢复提醒 ==========<br>
告警程序: {{ .Labels.job }} <br>
告警名称：{{ .Labels.alertname }}<br>
告警级别：{{ .Labels.severity }}<br>
告警机器：{{ .Labels.instance }}<br>
告警详情：{{ .Annotations.summary }}<br>
告警时间：{{ (.StartsAt.Add 28800e9).Format "2006-01-02 15:04:05" }}<br>
恢复时间：{{ (.EndsAt.Add 28800e9).Format "2006-01-02 15:04:05" }}<br>
========= END ==========<br>
{{- end }}
{{- end }}
{{- end }}

```

### 7. grafana

```
1). 默认账号/密码：admin/admin；
2). General/Home，选择DATA SOURCES；
3). 选择Prometheus；
4). URL项输入：http://ip:9090/；
5). 点击Save & test；
6). 在Data Sources/Prometheus，选择Prometheus 2.0 Stats，点击Import；
7). 在General/Home，选择Prometheus 2.0 Stats，就可以看到监控资源情况；
另：
如果需要监控其他的端口，第一步需要在prometheus.yml，添加对应的端口，再在Grafana下，+号中选择Import，添加对应的id（8919：Linux服务器监控，9276：Linux服务器监控，193：docker监控（需要部署docker监控端口（参考如下）））
部署docker监控端口：docker run -d --volume=/:/rootfs:ro --volume=/var/run:/var/run:ro  --volume=/sys:/sys:ro --volume=/var/lib/docker/:/var/lib/docker:ro  --volume=/dev/disk/:/dev/disk:ro --publish=8088:8080 --detach=true --name=cadvisor google/cadvisor:latest
```

### 安装参考
```
https://zhuanlan.zhihu.com/p/425304902
http://www.yunweipai.com/39494.html
```
