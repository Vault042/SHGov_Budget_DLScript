# SHGov_Budget_DLScript (上海市政府预算文档下载脚本)

本仓库包含一个用于自动化抓取和下载上海市人民政府官方网站（上海概览-政务公开）上各部门**预算公开文档（PDF格式）**的 Python 脚本。

该脚本会自动遍历各个大类（如党委部门、政府部门、人大政协等），抓取对应的文档链接，并根据部门分类自动创建本地文件夹，将下载的 PDF 文件规范重命名并整理归档。

## 🌟 功能特性 (Features)

* **动态网页支持**：使用 `DrissionPage` 驱动底层浏览器，完美绕过并解析需要动态加载渲染的政务网页。
* **自动化目录管理**：根据抓取到的“主分类（如政府部门）”和“子分类（如具体局委办）”，在本地自动创建层级目录。
* **断点续爬设计**：抓取流程被拆分为“获取部门链接”、“获取PDF链接”、“下载PDF”三个独立步骤，中间数据保存在 CSV 文件中。即使下载中断，也可以从现有的 CSV 数据继续运行。
* **健壮的下载机制**：使用 `requests.Session` 保持连接，配合文件流块级写入（Chunking）优化大文件下载内存占用；内置异常捕获和下载延时（休眠3秒），防止对目标服务器造成过大压力。
* **交互式运行菜单**：提供简单的命令行交互，用户可自由选择执行全流程或特定步骤。

## 🛠️ 环境要求与安装 (Prerequisites)

1.  **Python 3.7+**：请确保您的计算机上已安装 Python。
2.  **Chrome/Chromium 浏览器**：`DrissionPage` 需要依赖本地的 Chrome 或 Chromium 浏览器环境进行网页渲染。
3.  **第三方依赖包**：
    您可以通过以下命令安装必要的 Python 库：

    ```bash
    pip install requests lxml DrissionPage
    ```

## 🚀 使用指南 (Usage)

1.  克隆或下载本项目到本地。
2.  在终端或命令行中进入项目根目录。
3.  运行主脚本：

    ```bash
    python main_V2.py
    ```

4.  根据屏幕上的提示，输入对应的数字选择操作模式：
    * **输入 `1` (遍历所有步骤)**：从头开始抓取部门链接，随后抓取 PDF 下载地址，最后下载所有 PDF 到本地。
    * **输入 `2` (获取PDF下载地址并下载)**：假设 `files/two.csv` 已经存在，脚本将跳过最顶层的部门页面抓取，直接进入各部门页面获取文档链接并下载。
    * **输入 `3` (单独下载PDF)**：假设 `files/pdf.csv` 已经存在，仅执行最后一步，根据现有的链接列表读取并下载 PDF 文件。

## 📁 目录与文件结构说明 (Data Structure)

脚本运行后，会在当前目录下自动生成一个 `files/` 文件夹，内部结构如下：

```text
├── files/
│   ├── two.csv         # 存放第一步抓取到的二级部门页面链接 (分类, 部门名称, 部门URL)
│   ├── pdf.csv         # 存放第二步抓取到的PDF真实下载链接 (分类, 部门名称, PDF文件名, PDF下载URL)
│   ├── 党委部门/        # 自动生成的分类文件夹
│   │   ├── 某某委员会/   # 自动生成的具体部门文件夹
│   │   │   └── 2024年某某部门预算.pdf
│   ├── 政府部门/
│   └── ...
```

## ⚠️ 免责声明 (Disclaimer)

1. 仅供学习交流：本脚本仅用于个人学习 Python 爬虫技术、研究或自动化收集公开政务数据的合法用途，禁止用于任何商业用途或非法盈利。
2. 数据版权：脚本抓取的所有预算公开文档及数据版权均归属上海市人民政府及相关政府部门所有。
3. 合规使用：请在使用本脚本时严格遵守相关法律法规，以及目标网站的 robots.txt 规则。请保留代码中内置的延时机制，请勿修改脚本进行高并发、破坏性的恶意请求，以免对政务网站服务器造成负担。
4. 风险自负：因不当使用本脚本（如高频抓取导致 IP 被封禁、引发法律纠纷等）产生的一切后果，由使用者自行承担，本仓库及代码原作者概不负责。
- 因工作需要的自用爬虫仓库，可以自动化爬取“一网通办”上公开的预算信息

## 🫠 其他想说的
- 目前年份写死在代码里，因为我懒得获取并规格化用户输入
- Make it work, make it right, make it fast
- 用到的包在requirements.txt中，使用 `pip install -r requirements.txt` 安装
- **强烈建议使用虚拟环境**；当我还在曼哈顿技校当TA的时候，我的syllabus模板中一直写着这么一句：“if your jupyter notebook does not contain virtual envirionment related code, the highest score of your assignment will be fixed at 60.”

## 📍 Update Roadmap
- 规格化用户输入，并自动化替换URL中年份信息
- 下载的PDF接入LLM大模型 ╰(*°▽°*)╯：虽然不知道接入能干嘛，可能因为听起来很酷吧

## ⚠️ Attention
This repository is for personal usage only. I make it open-sourced just because it is cool and make sync much easier.

This is an open-sourced python file and it's only 150 lines long. OF COURSE it comes with NO WARRANTY.

If you want to imporve it, **READ THE GODDAMN CODE and create a fork**.

Someties I work jobs for living, sometimes I contribute pro bono to free and open source software projects, often I do both.
