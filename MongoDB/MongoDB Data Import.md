# MongoDB - Data Import

Created by : Mr Dk.

2019 / 03 / 07 15:15

Nanjing, Jiangsu, China

---

## About

**mongorestore** tool - 导入已有数据库的导出备份：

* 元数据为 `xxx.metadata.json`
* 数据为 `xxx.bson`

**mongoimport** tool - 导入单独的 Collection 的 JSON 导出文件

---

## Import

```console
$ mongorestore -d <db_name> /dir/...
```

```console
$ mongoimport --db <db_name> --collection <cl_name> --file data.json
```

---

