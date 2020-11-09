## Nacos Consul Adapter (for Prometheus)
当使用Nacos作为注册中心时通过`nacos-consul-adapter`能够使prometheus自动发现Nacos中的服务

## Restrictions
这个适配器只实现了prometheus使用consul_sd_config配置时需要的http接口，具体实现的接口如下：
- /v1/agent/self 返回默认的datacenter
- /v1/catalog/services 返回nacos中的服务列表
- /v1/catalog/service/{service} 返回服务实例
- /v1/health/service/{service} 返回服务heath情况

## Isuues
服务报错:将Prometheus版本降到v2.16.0，试了一个v2.13.0版本也不可以，最新版本也不可以

## Requirements
- Java 1.8+
- Spring Boot 2.1.x
- Spring Cloud Greenwich

## Prometheus
在prometheus配置文件中使用`consul_sd_configs`配置adapter地址

```
- job_name: 'nacos-prometheus'
  scrape_interval: 10s
  metrics_path: '/actuator/prometheus'
  consul_sd_configs:
  - server: 'localhost:8080'
    services: []
  relabel_configs:
      - source_labels: ['__meta_consul_service']
        target_label: 'service'
```

## 参考项目
[eureka-consule-adapter](https://github.com/twinformatics/eureka-consul-adapter)