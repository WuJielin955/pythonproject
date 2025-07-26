# Python 接口自动化测试框架

本项目是一个基于 `pytest + yaml + allure` 实现的轻量级接口自动化测试框架，具备用例管理、参数提取、断言校验、日志记录、测试报告生成等完整功能，适合中小型接口测试项目的快速落地与维护。

---

## 📁 项目目录结构说明

```

pythonproject/
├── base/           # 基础类封装，测试用例工具（如请求封装、断言、数据提取等）
├── common/         # 公共方法封装，如自定义函数、加密、时间处理等
├── conf/           # 全局配置文件目录（如 config.ini）
├── data/           # 测试数据存放目录（如 Excel、yaml 参数文件）
├── logs/           # 自动生成的日志文件目录
├── report/         # 测试报告目录（支持多种报告形式）
├── testcase/       # 用例编写目录，支持 YAML 格式
├── venv/           # Python 虚拟环境（建议使用外部环境以避免依赖冲突）
├── conftest.py     # pytest 全局钩子配置（固定文件名）
├── environment.xml # Allure 报告的环境信息（推荐使用 xml 显示中文）
├── extract.yaml    # 接口依赖参数提取存储
├── pytest.ini      # pytest 配置文件（固定文件名，禁止加中文注释）
├── requirements.txt# 第三方库依赖清单
└── run.py          # 主程序入口，执行测试的统一入口

````

---

## 🔧 安装依赖

推荐使用国内镜像源安装依赖：

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt
````

> ⚠️ 如使用本项目自带的 venv 虚拟环境，注意与本地 Python 版本兼容问题。若出现第三方库不兼容报错，请卸载后重新安装相应库。

---

## 🚀 使用方式

### 1. 编写测试用例（YAML）

每个 YAML 用例必须包含以下两个关键字：

* `baseInfo`: 接口基本信息（如地址、请求方式、请求头、依赖参数等）
* `testCase`: 测试场景定义，支持数据参数化、文件上传、断言校验、参数提取等

✅ 示例结构如下：

```yaml
baseInfo:
  api_name: 轨迹查询
  url: /monitor/vehicle/getMileageFrom
  method: post
  header:
    Content-Type: application/x-www-form-urlencoded;charset=UTF-8
    token: ${get_extract_data(token)}
    userid: ${get_extract_data(userid)}

testCase:
  -
    case_name: 有效车牌号轨迹查询
    data:
      vno: 鲁E00098
      color: 2
      startDate: ${start_time()}
      ...
    validation:
      - contains: {status_code: 200}
      - eq: {'state': '已入网'}
    extract:
      id: $.data
```

> JSONPath 在线解析工具：[点击打开](http://www.atoolbox.net/Tool.php?Id=792)

---

## 🧪 执行测试

运行主程序：

```bash
python run.py
```

或直接执行：

```bash
pytest
```

---

## 📊 测试报告 Allure

1. 生成报告数据：

   ```bash
   pytest --alluredir=report/temp
   ```

2. 生成可视化报告：

   ```bash
   allure generate report/temp -o report/html --clean
   ```

3. 启动本地服务查看报告：

   ```bash
   allure open report/html
   ```

---

## ✅ 参数说明 & 注意事项

* `url`：仅写接口路径，IP 和端口统一配置在 `conf/config.ini` 的 `[api_envi]`
* 参数引用格式：`${函数名(参数)}`，例如 `${get_extract_data(token)}`
* 参数类型：根据接口情况填写 `params`、`data`、或 `json` 三选一
* `pytest.ini` 和 `conftest.py` 文件名称不可修改，文件内不要写中文注释
* 项目中除 `data/` 外，其它目录下文件不可随意删除

---

## ⚙️ 其他工具

* **接口用例快速生成器**：可运行 `base/new_testcase_tools.py`，通过 UI 表单快速生成用例
* **测试环境说明文件**：可使用 `environment.xml`（推荐）或 `environment.properties` 提供 Allure 环境信息展示

---

## 🧠 常见问题

### Allure 报告未生成？

请检查：

* 是否正确安装 Allure 命令行工具
* 是否执行了 `pytest --alluredir=...`
* 报告目录是否有权限或被清空

更多帮助请参考：[Allure配置说明](https://app.yinxiang.com/fx/fd13ff11-369f-4b3b-bac9-c7ea18bd2f47)

---

## 👤 作者信息

* 📌 作者：吴杰林
* 📫 GitHub: [WuJielin955](https://github.com/WuJielin955)
* 📧 邮箱：m13426889145_1@163.com

---

## 📜 参考与致谢

* 项目设计参考多种企业级接口测试框架结构，结合自身实践进行封装优化
* 感谢所有开源工具及文档贡献者

---

> 📌 本框架适用于接口自动化测试入门、功能覆盖验证、参数提取依赖场景的快速实现。如有建议或反馈，欢迎提 Issue 或 PR 🙌
