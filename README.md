# uv 常用命令速查

**作者：刘老师**  
**金融科技创新实验室（FintechLab）**

本仓库是课堂演示用 Python 项目模板，主要帮助大家掌握 `uv` 的基本命令。后续课程中的 Python 代码、实验作业和项目练习，都建议优先使用 `uv` 管理环境和依赖。

`uv` 是一个现代 Python 项目管理工具，可以用来创建项目、管理依赖、同步环境、运行代码和导出依赖文件。相比传统的 `pip + venv` 组合，`uv` 的优势是速度快、命令统一、环境管理更清晰。

官方文档：

https://docs.astral.sh/uv/

---

## 1. 安装 uv

### macOS / Linux

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### Windows PowerShell

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

安装完成后，可以检查版本：

```bash
uv --version
```

---

## 2. 创建一个新的 Python 项目

```bash
uv init 项目名
```

例如：

```bash
uv init my_project
cd my_project
```

或者，也可以在当前vs code的项目（即：文件夹）中初始化项目（注意：要先进入当前项目里，再执行init）：

```bash
cd my_project
uv init
```

执行后，通常会生成这些文件：

```text
.
├── .python-version
├── README.md
├── main.py
├── pyproject.toml
└── uv.lock
```

其中：

- `pyproject.toml`：项目配置文件，记录项目名称、版本、依赖包等信息
- `.python-version`：记录当前项目使用的 Python 版本
- `main.py`：默认示例代码文件
- `uv.lock`：锁定依赖版本，保证不同电脑上安装出的环境一致
- `.venv`：虚拟环境目录，通常由 `uv sync` 或 `uv run` 自动创建

---

## 3. 运行 Python 文件

```bash
uv run python 文件名.py
```

例如：

```bash
uv run python main.py
```

推荐大家优先使用 `uv run`，这样不需要手动激活虚拟环境，`uv` 会自动使用当前项目对应的环境。

---

## 4. 安装第三方包

```bash
uv add 包名
```

例如安装 `requests`：

```bash
uv add requests
```

安装指定版本：

```bash
uv add requests==2.31.0
```

安装多个包：

```bash
uv add numpy pandas matplotlib scikit-learn
```

安装完成后，`uv` 会自动把依赖写入 `pyproject.toml`，并更新 `uv.lock`。

---

## 5. 卸载第三方包

```bash
uv remove 包名
```

例如：

```bash
uv remove requests
```

这会从项目依赖中移除该包，并同步更新配置文件。

---

## 6. 同步项目依赖

如果你从 GitHub 克隆了别人的 `uv` 项目，通常只需要执行：

```bash
uv sync
```

这个命令会根据 `pyproject.toml` 和 `uv.lock` 自动安装项目所需的所有依赖。

典型使用场景：

```bash
git clone 项目地址
cd 项目文件夹
uv sync
uv run python main.py
```

---

## 7. 查看已安装的包

查看当前环境中安装了哪些包：

```bash
uv pip list
```

查看某个包的详细信息：

```bash
uv pip show 包名
```

例如：

```bash
uv pip show requests
```

---

## 8. 查看项目依赖树

```bash
uv tree
```

这个命令可以查看当前项目中各个依赖包之间的关系。

---

## 9. 导出 requirements.txt

如果需要生成传统的 `requirements.txt` 文件，可以使用：

```bash
uv export > requirements.txt
```

这样可以方便和不使用 `uv` 的同学或平台兼容。

---

## 10. 常用命令总结

| 命令 | 作用 |
|---|---|
| `uv init` | 在当前目录创建 uv 项目 |
| `uv init 项目名` | 创建一个新的 uv 项目文件夹 |
| `uv add 包名` | 安装依赖包，并写入项目配置 |
| `uv add 包名==版本号` | 安装指定版本的依赖包 |
| `uv add 包1 包2 包3` | 一次安装多个依赖包 |
| `uv remove 包名` | 卸载依赖包 |
| `uv sync` | 根据配置文件同步安装所有依赖 |
| `uv run python 文件名.py` | 在 uv 管理的环境中运行 Python 文件 |
| `uv pip list` | 查看已安装的包 |
| `uv pip show 包名` | 查看某个包的详细信息 |
| `uv tree` | 查看项目依赖树 |
| `uv export > requirements.txt` | 导出 requirements.txt |

---

## 11. 推荐使用流程

### 新建项目时

```bash
uv init my_project
cd my_project
uv add requests pandas numpy
uv run python main.py
```

### 克隆已有项目时

```bash
git clone 项目地址
cd 项目文件夹
uv sync
uv run python main.py
```

### 添加新依赖时

```bash
uv add 包名
```

### 删除不需要的依赖时

```bash
uv remove 包名
```

### 项目运行前同步环境

```bash
uv sync
```

---

## 12. 注意事项

1. 不建议手动修改 `.venv` 文件夹。
2. 不建议把 `.venv` 上传到 GitHub。
3. 推荐把 `pyproject.toml` 和 `uv.lock` 都上传到 GitHub。
4. 如果项目运行报错，优先尝试：

```bash
uv sync
```

5. 运行代码时，推荐使用：

```bash
uv run python 文件名.py
```

而不是直接使用：

```bash
python 文件名.py
```

这样可以避免使用错 Python 环境。

---

## 13. 一个最小示例

创建项目：

```bash
uv init demo
cd demo
```

安装依赖：

```bash
uv add requests
```

新建 `test01.py`：

```python
import requests

response = requests.get("https://www.baidu.com")
print(response.status_code)
```

运行代码：

```bash
uv run python test01.py
```

如果输出类似：

```text
200
```

说明代码运行成功。

---

## 14. 常见问题

### Q1：为什么我明明安装了包，但是运行代码时提示找不到？

可能是因为你没有使用 `uv run` 运行代码。

推荐：

```bash
uv run python 文件名.py
```

不推荐：

```bash
python 文件名.py
```

---

### Q2：为什么不要上传 `.venv` 文件夹？

因为 `.venv` 是本地虚拟环境，里面文件很多，而且不同电脑、不同系统可能不兼容。

上传 GitHub 时，通常只需要上传：

```text
pyproject.toml
uv.lock
README.md
代码文件
```

不要上传：

```text
.venv
```

---

### Q3：别人下载我的项目后，怎么安装依赖？

进入项目文件夹后执行：

```bash
uv sync
```

然后运行代码：

```bash
uv run python 文件名.py
```

---

### Q4：什么时候用 `uv add`，什么时候用 `uv sync`？

安装新包时使用：

```bash
uv add 包名
```

同步已有依赖时使用：

```bash
uv sync
```

简单理解：

- `uv add`：我要给项目增加一个新包
- `uv sync`：我要根据项目配置，把环境恢复到正确状态

---

## 15. 建议大家记住的 5 个命令

初学阶段，先记住这 5 个命令就够了：

```bash
uv init
uv add 包名
uv remove 包名
uv sync
uv run python 文件名.py
```

掌握这几个命令后，就可以完成大部分 Python 项目的环境管理和代码运行。
