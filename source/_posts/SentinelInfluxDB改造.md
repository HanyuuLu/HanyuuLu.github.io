---
title: Sentinel开源版接入InfluxDB数据持久化并使用Grafana可视化
date: 2021-01-22
tags:
  - Sentinel
  - InfluxDB
  - Grafana
---

[toc]

# 前言

[Sentinel开源版本](https://github.com/alibaba/Sentinel)流量数据保存在内存中，有效期为最近5min，为了能让数据持久化保存并进行中长期分析，参照了一些文章，我们对Sentinel-dashboard流量数据进行改造、接入InfluxDB。

>   为了阅读上更直观，本文里所有URL已做反转义处理，另，host名为杜撰，改造时请自行带入实际的hostname或ip

## 准备工作

1.  Sentinel[源代码](https://github.com/alibaba/Sentinel)

    ``` bash
    git pull git@github.com:alibaba/Sentinel.git
    ```

2.  InfluxDB 1.8 Docker镜像

    ```bash
    docker pull influxdb:latest
    # 笔者从DockerHub拉取时最新的镜像版本为1.8.3，如2.x版本无法使用请使用下方拉取1.8.3版本
    docker pull influxdb:1.8.3
    ```

# 接入InfluxDB

1.  SentinelDB环境准备

>   安装和权限配置本文不再展开，详细请参阅[create database sentinel_db](https://hanyuu.icu/InfluxDB%20%E9%85%8D%E7%BD%AE%E7%94%A8%E6%88%B7%E6%9D%83%E9%99%90%EF%BC%88Docker%EF%BC%89)

创建数据库以备使用，在influx命令行或者HTTP调用中

*   cli

``` bash
> create database sentinel_db
```

*   HTTP call

```
[POST] http://influxdb.idc.hanyuu.demo:8086/query?q=create database sentinel_db
```

2.  在`pom.xml`中、添加InfluxDB和Lombok（简化开发流程）依赖

    ```xml
            <!--
                lqlu03
                使用infulxdb作为数据持久化数据库
            -->
            <dependency>
                <groupId>org.influxdb</groupId>
                <artifactId>influxdb-java</artifactId>
                <version>2.17</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>1.18.16</version>
                <scope>provided</scope>
            </dependency>
    ```

    

3.  添加数据库工具类

    ```java
    package com.alibaba.csp.sentinel.dashboard.util;
    
    import java.util.List;
    import java.util.Map;
    import java.util.Set;
    
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.Getter;
    import lombok.extern.slf4j.Slf4j;
    import org.influxdb.InfluxDB;
    import org.influxdb.InfluxDBFactory;
    import org.influxdb.dto.BoundParameterQuery;
    import org.influxdb.dto.QueryResult;
    import org.influxdb.impl.InfluxDBResultMapper;
    import org.springframework.beans.factory.annotation.Value;
    import org.springframework.stereotype.Component;
    
    /**
     * @author HanyuuLu
     * @date 2020-01-14
     *
     * Sentinel
     */
    @Data
    @AllArgsConstructor
    @Slf4j
    @Component
    public class InfluxDBUtils {
    
        private static String url;
    
        private static String username;
    
        private static String password;
    
        private static InfluxDBResultMapper resultMapper = new InfluxDBResultMapper();
    
        @Value("${influxdb.url}")
        public void setUrl(String url) {
            InfluxDBUtils.url = url;
        }
    
        @Value("${influxdb.username}")
        public void setUsername(String username) {
            InfluxDBUtils.username = username;
        }
    
        @Value("${influxdb.password}")
        public void setPassword(String password) {
            InfluxDBUtils.password = password;
        }
    
        public static <T> T process(String database, InfluxDBCallback callback) {
            InfluxDB influxDb = null;
            T t = null;
            try {
                influxDb = InfluxDBFactory.connect(url, username, password);
                influxDb.setDatabase(database);
    
                t = callback.doCallBack(database, influxDb);
            } catch (Exception e) {
                log.error("[process exception]", e);
            } finally {
                if (influxDb != null) {
                    try {
                        influxDb.close();
                    } catch (Exception e) {
                        log.error("[influxDB.close exception]", e);
                    }
                }
            }
    
            return t;
        }
    
        public static void insert(String database, InfluxDBInsertCallback influxDBInsertCallback) {
            process(database, new InfluxDBCallback() {
                @Override
                public <T> T doCallBack(String database, InfluxDB influxDB) {
                    influxDBInsertCallback.doCallBack(database, influxDB);
                    return null;
                }
            });
    
        }
    
        public static QueryResult query(String database, InfluxDBQueryCallback influxDBQueryCallback) {
            return process(database, new InfluxDBCallback() {
                @Override
                public <T> T doCallBack(String database, InfluxDB influxDB) {
                    QueryResult queryResult = influxDBQueryCallback.doCallBack(database, influxDB);
                    return (T) queryResult;
                }
            });
        }
    
        public static <T> List<T> queryList(String database, String sql, Map<String, Object> paramMap, Class<T> clasz) {
            QueryResult queryResult = query(database, new InfluxDBQueryCallback() {
                @Override
                public QueryResult doCallBack(String database, InfluxDB influxDB) {
                    BoundParameterQuery.QueryBuilder queryBuilder = BoundParameterQuery.QueryBuilder.newQuery(sql);
                    queryBuilder.forDatabase(database);
    
                    if (paramMap != null && paramMap.size() > 0) {
                        Set<Map.Entry<String, Object>> entries = paramMap.entrySet();
                        for (Map.Entry<String, Object> entry : entries) {
                            queryBuilder.bind(entry.getKey(), entry.getValue());
                        }
                    }
    
                    return influxDB.query(queryBuilder.create());
                }
            });
    
            return resultMapper.toPOJO(queryResult, clasz);
        }
    
        public interface InfluxDBCallback {
            <T> T doCallBack(String database, InfluxDB influxDB);
        }
    
        public interface InfluxDBInsertCallback {
            void doCallBack(String database, InfluxDB influxDB);
        }
    
        public interface InfluxDBQueryCallback {
            QueryResult doCallBack(String database, InfluxDB influxDB);
        }
    }
    ```

url、username、password用于存储InfluxDB的连接、用户名、密码信息，定义为static属性，因此在set方法上使用@Value注解从配置文件读取属性值；

*   resultMapper用于查询结果到实体类的映射；

*   init方法用于初始化url、username、password；

*   process为通用的处理方法，负责打开关闭连接，并且调用InfluxDBCallback回调方法；

*   insert为插入数据方法，配合InfluxDBInsertCallback回调使用；

*   query为通用的查询方法，配合InfluxDBQueryCallback回调方法使用，返回QueryResult对象；

*   queryList为查询列表方法，调用query得到QueryResult，再通过resultMapper转换为List<实体类>;    

在resources目录下的application.properties文件中，增加InfluxDB的配置： 

  ```properties
  influxdb.url=http://influxdb.idc.hanyuu.demo:8086
  influxdb.username=username
  influxdb.password=p@ssw0rd
  ```

4.  添加实体类

     ``` java
    package com.alibaba.csp.sentinel.dashboard.datasource.entity.InfluxDb;
    
    
    import java.time.Instant;
    
    import org.influxdb.annotation.Column;
    import org.influxdb.annotation.Measurement;
    
    import lombok.Data;
    
    /**
     * @author HanyuuLu
     * @date 2020-01-14
     */
    @Data
    @Measurement(name = "sentinel_metric")
    public class MetricPO {
    
        @Column(name = "time")
        private Instant time;
    
        @Column(name = "id")
        private Long id;
    
        @Column(name = "gmtCreate")
        private Long gmtCreate;
    
        @Column(name = "gmtModified")
        private Long gmtModified;
    
        @Column(name = "app", tag = true)
        private String app;
    
        @Column(name = "resource", tag = true)
        private String resource;
    
        @Column(name = "passQps")
        private Long passQps;
    
        @Column(name = "successQps")
        private Long successQps;
    
        @Column(name = "blockQps")
        private Long blockQps;
    
        @Column(name = "exceptionQps")
        private Long exceptionQps;
    
        @Column(name = "rt")
        private double rt;
    
        @Column(name = "count")
        private int count;
    
        @Column(name = "resourceCode")
        private int resourceCode;
    }
     ```

    该类参考MetricEntity创建，加上influxdb-java包提供的注解，通过@Measurement(name = "sentinel_metric")指定数据表(measurement)名称，

    

    time作为时序数据库的时间列；

    app、resource设置为tag列，通过注解标识为tag=true；

    其它字段为filed列；

5.  实现`MetricsRepository`接口

在如下package中，找到接口`public interface MetricsRepository<T>`,这是一个关于指标数据保存和应用信息查询的借口，我们从这里重写方法以保存到InfluxDB。

``` java
package com.alibaba.csp.sentinel.dashboard.repository.metric;
```

对接口重新进行实现

``` java
package com.alibaba.csp.sentinel.dashboard.repository.metric;

import com.alibaba.csp.sentinel.util.StringUtil;
import com.alibaba.csp.sentinel.dashboard.datasource.entity.MetricEntity;
import com.alibaba.csp.sentinel.dashboard.datasource.entity.InfluxDb.MetricPO;
import com.alibaba.csp.sentinel.dashboard.util.InfluxDBUtils;
import org.apache.commons.lang.time.DateFormatUtils;
import org.apache.commons.lang.time.DateUtils;
import org.influxdb.InfluxDB;
import org.influxdb.dto.Point;
import org.springframework.stereotype.Repository;
import org.springframework.util.CollectionUtils;

import java.util.*;
import java.util.concurrent.TimeUnit;
import java.util.stream.Collectors;

/**
 * metrics数据InfluxDB存储实现
 * @author HanyuuLu
 * @date 2020-01-14
 */
@Repository("influxDBMetricsRepository")
public class InfluxDBMetricsRepository implements MetricsRepository<MetricEntity> {

    /**时间格式*/
    private static final String DATE_FORMAT_PATTERN = "yyyy-MM-dd HH:mm:ss.SSS";

    /**数据库名称*/
    private static final String SENTINEL_DATABASE = "sentinel_db";

    /**数据表名称*/
    private static final String METRIC_MEASUREMENT = "sentinel_metric";

    /**北京时间领先UTC时间8小时 UTC: Universal Time Coordinated,世界统一时间*/
    private static final Integer UTC_8 = 8;

    @Override
    public void save(MetricEntity metric) {
        if (metric == null || StringUtil.isBlank(metric.getApp())) {
            return;
        }

        InfluxDBUtils.insert(SENTINEL_DATABASE, new InfluxDBUtils.InfluxDBInsertCallback() {
            @Override
            public void doCallBack(String database, InfluxDB influxDB) {
                if (metric.getId() == null) {
                    metric.setId(System.currentTimeMillis());
                }
                doSave(influxDB, metric);
            }
        });
    }

    @Override
    public void saveAll(Iterable<MetricEntity> metrics) {
        if (metrics == null) {
            return;
        }

        Iterator<MetricEntity> iterator = metrics.iterator();
        boolean next = iterator.hasNext();
        if (!next) {
            return;
        }

        InfluxDBUtils.insert(SENTINEL_DATABASE, new InfluxDBUtils.InfluxDBInsertCallback() {
            @Override
            public void doCallBack(String database, InfluxDB influxDB) {
                while (iterator.hasNext()) {
                    MetricEntity metric = iterator.next();
                    if (metric.getId() == null) {
                        metric.setId(System.currentTimeMillis());
                    }
                    doSave(influxDB, metric);
                }
            }
        });
    }

    @Override
    public List<MetricEntity> queryByAppAndResourceBetween(String app, String resource, long startTime, long endTime) {
        List<MetricEntity> results = new ArrayList<MetricEntity>();
        if (StringUtil.isBlank(app)) {
            return results;
        }

        if (StringUtil.isBlank(resource)) {
            return results;
        }

        StringBuilder sql = new StringBuilder();
        sql.append("SELECT * FROM " + METRIC_MEASUREMENT);
        sql.append(" WHERE app=$app");
        sql.append(" AND resource=$resource");
        sql.append(" AND time>=$startTime");
        sql.append(" AND time<=$endTime");

        Map<String, Object> paramMap = new HashMap<String, Object>();
        paramMap.put("app", app);
        paramMap.put("resource", resource);
        paramMap.put("startTime", DateFormatUtils.format(new Date(startTime), DATE_FORMAT_PATTERN));
        paramMap.put("endTime", DateFormatUtils.format(new Date(endTime), DATE_FORMAT_PATTERN));

        List<MetricPO> metricPOS = InfluxDBUtils.queryList(SENTINEL_DATABASE, sql.toString(), paramMap, MetricPO.class);

        if (CollectionUtils.isEmpty(metricPOS)) {
            return results;
        }

        for (MetricPO metricPO : metricPOS) {
            results.add(convertToMetricEntity(metricPO));
        }

        return results;
    }

    @Override
    public List<String> listResourcesOfApp(String app) {
        List<String> results = new ArrayList<>();
        if (StringUtil.isBlank(app)) {
            return results;
        }

        StringBuilder sql = new StringBuilder();
        sql.append("SELECT * FROM " + METRIC_MEASUREMENT);
        sql.append(" WHERE app=$app");
        sql.append(" AND time>=$startTime");

        Map<String, Object> paramMap = new HashMap<String, Object>();
        long startTime = System.currentTimeMillis() - 1000 * 60;
        paramMap.put("app", app);
        paramMap.put("startTime", DateFormatUtils.format(new Date(startTime), DATE_FORMAT_PATTERN));

        List<MetricPO> metricPOS = InfluxDBUtils.queryList(SENTINEL_DATABASE, sql.toString(), paramMap, MetricPO.class);

        if (CollectionUtils.isEmpty(metricPOS)) {
            return results;
        }

        List<MetricEntity> metricEntities = new ArrayList<MetricEntity>();
        for (MetricPO metricPO : metricPOS) {
            metricEntities.add(convertToMetricEntity(metricPO));
        }

        Map<String, MetricEntity> resourceCount = new HashMap<>(32);

        for (MetricEntity metricEntity : metricEntities) {
            String resource = metricEntity.getResource();
            if (resourceCount.containsKey(resource)) {
                MetricEntity oldEntity = resourceCount.get(resource);
                oldEntity.addPassQps(metricEntity.getPassQps());
                oldEntity.addRtAndSuccessQps(metricEntity.getRt(), metricEntity.getSuccessQps());
                oldEntity.addBlockQps(metricEntity.getBlockQps());
                oldEntity.addExceptionQps(metricEntity.getExceptionQps());
                oldEntity.addCount(1);
            } else {
                resourceCount.put(resource, MetricEntity.copyOf(metricEntity));
            }
        }

        // Order by last minute b_qps DESC.
        return resourceCount.entrySet()
                .stream()
                .sorted((o1, o2) -> {
                    MetricEntity e1 = o1.getValue();
                    MetricEntity e2 = o2.getValue();
                    int t = e2.getBlockQps().compareTo(e1.getBlockQps());
                    if (t != 0) {
                        return t;
                    }
                    return e2.getPassQps().compareTo(e1.getPassQps());
                })
                .map(Map.Entry::getKey)
                .collect(Collectors.toList());
    }

    private MetricEntity convertToMetricEntity(MetricPO metricPO) {
        MetricEntity metricEntity = new MetricEntity();

        metricEntity.setId(metricPO.getId());
        metricEntity.setGmtCreate(new Date(metricPO.getGmtCreate()));
        metricEntity.setGmtModified(new Date(metricPO.getGmtModified()));
        metricEntity.setApp(metricPO.getApp());
        metricEntity.setTimestamp(Date.from(metricPO.getTime().minusMillis(TimeUnit.HOURS.toMillis(UTC_8))));
        metricEntity.setResource(metricPO.getResource());
        metricEntity.setPassQps(metricPO.getPassQps());
        metricEntity.setSuccessQps(metricPO.getSuccessQps());
        metricEntity.setBlockQps(metricPO.getBlockQps());
        metricEntity.setExceptionQps(metricPO.getExceptionQps());
        metricEntity.setRt(metricPO.getRt());
        metricEntity.setCount(metricPO.getCount());

        return metricEntity;
    }

    private void doSave(InfluxDB influxDB, MetricEntity metric) {
        influxDB.write(Point.measurement(METRIC_MEASUREMENT)
                .time(DateUtils.addHours(metric.getTimestamp(), UTC_8).getTime(), TimeUnit.MILLISECONDS)// UTC -> Shanghai(UTC+8) time zone.
                .tag("app", metric.getApp())
                .tag("resource", metric.getResource())
                .addField("id", metric.getId())
                .addField("gmtCreate", metric.getGmtCreate().getTime())
                .addField("gmtModified", metric.getGmtModified().getTime())
                .addField("passQps", metric.getPassQps())
                .addField("successQps", metric.getSuccessQps())
                .addField("blockQps", metric.getBlockQps())
                .addField("exceptionQps", metric.getExceptionQps())
                .addField("rt", metric.getRt())
                .addField("count", metric.getCount())
                .addField("resourceCode", metric.getResourceCode())
                .build());
    }
}
```

其中：

save、saveAll方法通过调用InfluxDBUtils.insert和InfluxDBInsertCallback回调方法，往sentinel_db库的sentinel_metric数据表写数据；

saveAll方法不是循环调用save方法，而是在回调内部循环Iterable<MetricEntity> metrics处理，这样InfluxDBFactory.connect连接只打开关闭一次；

doSave方法中，.time(DateUtils.addHours(metric.getTimestamp(), 8).getTime(), TimeUnit.MILLISECONDS)

因InfluxDB的UTC时间暂时没找到修改方法，所以这里time时间列加了8个小时时差；

queryByAppAndResourceBetween、listResourcesOfApp里面的查询方法，使用InfluxDB提供的类sql语法，编写查询语句即可。

2.  注册Spring Bean,使用`@Qualifier`指定要使用的bean name

在`com.alibaba.csp.sentinel.dashboard.controlle.MetricController`、``com.alibaba.csp.sentinel.dashboard.metric.MetricFetcher`类，找到metricStore属性，在@Autowired注解上面加上@Qualifier注解：

```java
    @Autowired
    @Qualifier("influxDBMetricsRepository")
    private MetricsRepository<MetricEntity> metricStore;
```

# 验证

1.  在需要的工程里接入Sentinel，并启动改造后的Sentinel dashboard

``` bash
mvn spring-boot:run [-Pdev]
```

证实项目正常启动，若否，通过git记录和上文核对问题并修正

2.  正常打开dashboard，确认有流量流入
3.  打开InfluxDB，发送HTTP请求查询`measurements`

```
[GET] http://influxdb.idc.hanyuu.demo:8086/query?pretty=true&db=sentinel_db&q=show measurements
```

证实measurement已被创建

``` json
{
  "results": [
    {
      "statement_id": 0,
      "series": [
        {
          "name": "measurements",
          "columns": [
            "name"
          ],
          "values": [
            [
              "sentinel_metric"
            ]
          ]
        }
      ]
    }
  ]
}
```

*   查询最新三条记录

``` 
[GET] http://influxdb.idc.hanyuu.demo:8086/query?pretty=true&db=sentinel_db&q=select * from sentinel_metric order by time limit 3
```

``` json
{
  "results": [
    {
      "statement_id": 0,
      "series": [
        {
          "name": "sentinel_metric",
          "columns": [
            "time",
            "app",
            "blockQps",
            "count",
            "exceptionQps",
            "gmtCreate",
            "gmtModified",
            "id",
            "passQps",
            "resource",
            "resourceCode",
            "rt",
            "successQps"
          ],
          "values": [
            [
              "2021-01-21T15:57:28Z",
              "com.github.hanyuulu.demo.demo.DemoApplication",
              0,
              1,
              0,
              1611215851663,
              1611215851663,
              1611215862422,
              1,
              "/wait",
              47047204,
              19,
              1
            ],
            [
              "2021-01-21T15:57:28Z",
              "com.github.hanyuulu.demo.demo.DemoApplication",
              0,
              1,
              0,
              1611215851663,
              1611215851663,
              1611215862399,
              1,
              "LONG_TIME_CONTROLLER",
              1822368907,
              4,
              1
            ],
            [
              "2021-01-21T15:57:29Z",
              "com.github.hanyuulu.demo.demo.DemoApplication",
              0,
              1,
              0,
              1611215851663,
              1611215851663,
              1611215856013,
              2,
              "/error",
              1442355001,
              44,
              2
            ]
          ]
        }
      ]
    }
  ]
}
```

# 接入Grafana

为了更直观的分析数据，我们接入Grafana进行可视化分析

1.  通过Docker拉取安装Grafana镜像

    ``` bash
    docker pull grafana/grafana
    docker run -d -p 3000:3000 --name grafana grafana/grafana
    ```

2.  使用默认账户密码`admin`、`admin`登陆，创建dashboard，自行创建需要的图表即可，下面简单介绍下各个参数的含义

    | field        | note               |
    | ------------ | ------------------ |
    | time         | 时间戳             |
    | app          | 接入应用[tag]      |
    | resource     | 资源名称[tag]      |
    | resourceCode | 资源code           |
    | id           | 聚合id             |
    | gmtCreate    | gmt创建时间        |
    | gmtModified  | gmt修改时间        |
    | count        | 本次聚合的总条数   |
    | successQps   | 成功QPS            |
    | passQps      | 通过QPS            |
    | blockQps     | 阻拦QPS            |
    | exceptionQPS | 异常QPS            |
    | rt           | 所有成功退出的计数 |

    # 参考文档

*   [sentinel控制台监控数据持久化【InfluxDB】](https://www.cnblogs.com/cdfive2018/p/9914838.html)

*   https://github.com/alibaba/Sentinel/wiki/控制台

*   https://github.com/alibaba/Sentinel/wiki/在生产环境中使用-Sentinel-控制台

*   https://docs.influxdata.com/influxdb/v1.6/introduction/getting-started/ InfluxDB官网文档

*   https://xtutu.gitbooks.io/influxdb-handbook/content/ InfluxDB简明手册

