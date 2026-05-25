---
name: testcase-exporter
description: 测试用例导出技能 - 将测试点JSON导出为Excel或XMind格式的测试用例文档
required_variables:
  - testpoints_json    # 测试点JSON文件路径（支持汇总文件或章节文件）
  - output_dir        # 输出目录路径
  - export_format     # 导出格式：excel | xmind | both（默认both）
  - filename          # 输出文件名（可选，默认自动生成）
---

# 测试用例导出技能 (Testcase Exporter)

## 1. 技能定位

你是一名资深测试工程师，擅长将测试点文档转换为标准化的测试用例文档。本技能专注于**阶段5：用例导出**，支持两种格式：

| 格式 | 说明 | 适用场景 |
|------|------|----------|
| **Excel** | 表格形式，适合用例评审和执行跟踪 | 日常测试执行 |
| **XMind** | 思维导图形式，适合需求梳理和用例设计 | 需求理解、设计讨论 |
| **both** | 同时导出两种格式 | 全面交付 |

**前置依赖**：testpoint-extractor 或 testpoint-reviewer 技能已执行完成

---

## 2. 输入输出规范

### 2.1 输入参数

| 参数              | 类型     | 说明                                       | 默认值 |
| ----------------- | -------- | ------------------------------------------ | ------ |
| `testpoints_json` | path     | 测试点 JSON 文件路径（支持汇总或章节文件） | -      |
| `output_dir`      | path     | 输出目录路径                               | -      |
| `export_format`   | string   | 导出格式：excel / xmind / both            | both   |
| `filename`        | string   | 输出文件名（可选）                         | 自动生成 |

### 2.2 支持的输入路径

```json
// 汇总文件
"04_testpoints/testpoints_summary.json"

// 章节文件
"04_testpoints/01_第3章_新品牌运价业务/testpoints.json"

// 评审后文件
"05_testpoint_review/01_第3章_新品牌运价业务/improved_testpoints.json"
```

### 2.3 输出目录结构

```
{output_dir}/
├── {项目名称}/
│   ├── 04_testpoints/
│   │   └── ...
│   ├── 05_testpoint_review/
│   │   └── ...
│   ├── 06_testcase_export/              # Excel 输出
│   │   ├── 国航电子商务项目二期_测试用例_第3章_20260411.xlsx
│   │   ├── 国航电子商务项目二期_测试用例_全部章节_20260411.xlsx
│   │   └── README.md
│   └── 07_xmind_export/                 # XMind 输出
│       ├── 国航电子商务项目二期_测试用例_第3章_20260411.xmind
│       ├── 国航电子商务项目二期_测试用例_全部章节_20260411.xmind
│       └── README.md
```

---

## 3. Excel 导出格式

### 3.1 标准列定义

| 列号 | 列名   | 说明                 | 必填 |
| -- | ---- | ------------------ | -- |
| A  | 用例ID | 唯一标识符              | ✅  |
| B  | 用例名称 | 测试用例名称             | ✅  |
| C  | 需求ID | 对应需求编号             | ✅  |
| D  | 需求名称 | 对应需求名称             | ✅  |
| E  | 测试类型 | FT/BT/ET/UIT/PT/ST | ✅  |
| F  | 优先级  | P0/P1/P2/P3        | ✅  |
| G  | 前置条件 | 测试执行的前置条件          | ✅  |
| H  | 测试步骤 | 详细的测试执行步骤          | ✅  |
| I  | 预期结果 | 预期输出和结果            | ✅  |
| J  | 测试数据 | 所需的测试数据            | ❌  |
| K  | 备注   | 补充说明               | ❌  |
| L  | 评审状态 | 已评审/待评审            | ❌  |

### 3.2 样式规范

```
表头行：
- 背景色：深蓝色 (RGB: 0, 56, 138)
- 字体色：白色
- 字体：加粗
- 高度：25px

数据行：
- 交替背景色：白色 / 浅蓝色 (RGB: 240, 245, 250)
- 字体色：黑色
- 自动换行
- 行高：根据内容自适应（最小20px）

边框：
- 所有单元格：实线边框
- 边框色：灰色 (RGB: 180, 180, 180)
```

### 3.3 Sheet命名规则

```
Sheet1：封面/目录
Sheet2：第3章_新品牌运价业务
Sheet3：第4章_常旅客直减功能
...
SheetN：汇总统计
```

### 3.4 封面 Sheet 结构

```excel
┌─────────────────────────────────────────────┐
│           测试用例文档                        │
├─────────────────────────────────────────────┤
│  项目名称：国航电子商务项目二期                │
│  导出格式：Excel                            │
│  用例总数：350                              │
│  生成日期：2026-04-11                       │
│  评审状态：已评审                             │
├─────────────────────────────────────────────┤
│  [目录]                                      │
│  Sheet2：第3章_新品牌运价业务 (80个用例)     │
│  Sheet3：第4章_常旅客直减功能 (41个用例)     │
└─────────────────────────────────────────────┘
```

### 3.5 汇总统计 Sheet

```excel
┌─────────────────────────────────────────────┐
│           测试用例统计                        │
├─────────────────────────────────────────────┤
│  总用例数：350                              │
│  功能测试 (FT)：280                         │
│  边界测试 (BT)：40                          │
│  异常测试 (ET)：20                          │
│  UI测试 (UIT)：10                           │
├─────────────────────────────────────────────┤
│  P0：50   P1：150   P2：100   P3：50       │
├─────────────────────────────────────────────┤
│  按章节统计：                                │
│  第3章：80个  第4章：41个  第5章：45个 ...   │
└─────────────────────────────────────────────┘
```

---

## 4. XMind 导出格式

### 4.1 XMind 文件本质

XMind 8 文件是一个 **ZIP 压缩包**：

```
{项目名}.xmind (实为ZIP)
├── content.json          # 思维导图内容（核心）
├── meta.xml              # 元数据
├── comment.xml           # 注释配置
├──衣橱/                  # 样式资源
│   └── metadata.json
└── attachments/          # 附件（图片等）
    └── resources/
```

### 4.2 content.json 结构

```json
{
  "type": "mindmap",
  "rootTopic": {
    "id": "root",
    "title": "{项目名称}测试用例",
    "children": {
      "topics": [
        {
          "id": "ch3",
          "title": "第3章 新品牌运价业务",
          "children": {
            "topics": [
              {
                "id": "req001",
                "title": "REQ-CH3-001 按FareFamily显示最低价格",
                "children": {
                  "topics": [
                    {
                      "id": "tp001",
                      "title": "[P0] 验证国内航线显示5个FareFamily",
                      "children": {
                        "topics": [
                          {
                            "id": "precond",
                            "title": "【前置条件】",
                            "notes": {
                              "plain": "1. 用户进入航班搜索结果页面\n2. 系统返回国内航线航班列表"
                            }
                          },
                          {
                            "id": "steps",
                            "title": "【测试步骤】",
                            "notes": {
                              "plain": "1. 在搜索结果页面查看国内航线航班列表\n2. 识别所有FareFamily分类\n3. 检查每个FareFamily组下的价格显示"
                            }
                          },
                          {
                            "id": "results",
                            "title": "【预期结果】",
                            "notes": {
                              "plain": "1. 国内航线显示5个FareFamily分类\n2. 每个FareFamily组下仅显示最低可用价格"
                            }
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```

### 4.3 层级结构

```
测试点 JSON              XMind 层级
─────────────────────────────────────────────────────
项目名称                   根节点 (Root Topic)
  │
  ├── chapter.chapter_title ──► 一级子节点 (Chapter)
  │       │
  │       ├── testpoints[].requirement_id ──► 二级子节点 (Requirement)
  │       │       │
  │       │       └── testpoints[].testpoint_id ──► 三级子节点 (Testpoint)
  │       │               │
  │       │               └── 测试点详情 ──► 四级子节点 (Details)
  │       │                       ├── 前置条件
  │       │                       ├── 测试步骤
  │       │                       └── 预期结果
```

### 4.4 节点命名规则

| 层级 | 节点类型 | title 格式                              | 颜色/图标  |
| ---- | -------- | --------------------------------------- | ---------- |
| 0    | 根节点   | {项目名称}测试用例                       | 蓝色       |
| 1    | 章节节点 | 第{章}章 {章节名}                        | 深蓝色     |
| 2    | 需求节点 | {需求ID} {需求名称}                      | 紫色       |
| 3    | 测试点   | [{优先级}] {测试点名称}                   | 按优先级   |
| 4    | 详情节点 | 【前置条件】/【测试步骤】/【预期结果】      | 灰色       |

### 4.5 优先级颜色映射

```json
{
  "P0": "#FF0000",  // 红色 - 核心功能
  "P1": "#FF6600",  // 橙色 - 重要功能
  "P2": "#FFCC00",  // 黄色 - 一般功能
  "P3": "#999999"   // 灰色 - 低优先级
}
```

### 4.6 XMind 封面结构

在根节点下添加统计概要：

```json
{
  "id": "summary",
  "title": "📊 测试用例统计",
  "children": {
    "topics": [
      { "id": "total", "title": "总用例数：350" },
      { "id": "by_type", "title": "FT:280 BT:40 ET:20 UIT:10" },
      { "id": "by_priority", "title": "P0:50 P1:150 P2:100 P3:50" }
    ]
  }
}
```

---

## 5. 执行流程

### 5.1 整体流程

```
1. 解析输入参数
   ├── 验证 testpoints_json 是否存在
   ├── 确定 export_format（excel/xmind/both）
   ├── 确定输出目录
   └── 生成文件名

2. 读取测试点数据
   ├── 解析 testpoints.json
   ├── 识别章节分组
   └── 按章节分组测试点

3. 执行导出
   ├── export_format 检查
   │
   ├── [excel] → 执行 Excel 导出流程
   │
   ├── [xmind] → 执行 XMind 导出流程
   │
   └── [both] → 先执行 Excel，再执行 XMind

4. 保存并验证
   ├── 保存文件
   ├── 验证文件完整性
   └── 输出文件路径
```

### 5.2 Excel 导出流程

```
Excel 流程：
1. 创建 workbook
2. 创建封面 Sheet（项目信息 + 目录）
3. 遍历章节创建 Sheet
4. 写入表头 + 数据行
5. 创建汇总统计 Sheet
6. 应用样式
7. 保存 .xlsx 文件
```

### 5.3 XMind 导出流程

```
XMind 流程：
1. 创建临时目录结构
2. 构建 content.json（递归 topics 树）
3. 生成 meta.xml
4. 生成 衣橱/metadata.json
5. 复制图片到 attachments/resources/
6. 打包为 ZIP（重命名为 .xmind）
7. 清理临时目录
```

---

## 6. 数据转换规则

### 6.1 JSON字段到Excel列的映射

```json
{
  "testpoint_id"        → A列（用例ID）
  "testpoint_name"      → B列（用例名称）
  "requirement_id"      → C列（需求ID）
  "requirement_name"    → D列（需求名称）
  "test_type"           → E列（测试类型）
  "priority"            → F列（优先级）
  "preconditions"       → G列（前置条件）
  "test_steps"          → H列（测试步骤）
  "expected_results"    → I列（预期结果）
  "test_data"           → J列（测试数据）
  "notes"               → K列（备注）
}
```

### 6.2 JSON字段到XMind节点的映射

```json
{
  "chapter.chapter_title" → 章节节点 title
  "requirement_id"        → 需求节点 title
  "requirement_name"       → 需求节点 title（附加信息）
  "testpoint_name"         → 测试点节点 title（带优先级前缀）
  "preconditions"          → 【前置条件】子节点 notes
  "test_steps"            → 【测试步骤】子节点 notes
  "expected_results"       → 【预期结果】子节点 notes
  "test_data"             → 合并到【前置条件】
  "test_type"             → 显示在测试点节点括号中
}
```

### 6.3 数组字段处理

| 字段 | Excel处理 | XMind处理 |
|------|---------- |----------|
| preconditions | 用换行符连接 | 用换行符连接写入 notes |
| test_steps | 保留序号，用换行符连接 | 保留序号，用换行符写入 notes |
| expected_results | 用换行符连接 | 用换行符连接写入 notes |
| test_data | 用分号连接 | 合并到【前置条件】 |
| related_images | - | 复制到 attachments/ |

---

## 7. 与其他Skill的协作

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Skill 协作流程                                  │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. requirement-splitter (拆分)                         ◄──┐         │
│     输出：03_requirements/                                       │         │
│                                                                      │
│  2. testpoint-extractor (提取)                      ───┐            │
│     读取：03_requirements/                               │            │
│     输出：04_testpoints/  ──────────────────────────────┼────────┐  │
│                                                        │        │  │
│  3. testpoint-reviewer (评审)          ◄────────────┘        │  │
│     读取：04_testpoints/                                     │        │
│     输出：05_testpoint_review/                              │        │
│                                                                      │  │
│  4. testcase-exporter (导出)  ◄────────────────────────────────────────┘
│     读取：05_testpoint_review/improved_testpoints.json
│     输出：06_testcase_export/ (Excel)
│     输出：07_xmind_export/ (XMind)
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

| Skill                   | 输出物           | 后续 Skill 使用              |
| ----------------------- | ---------------- | --------------------------- |
| requirement-splitter    | 需求 JSON        | 被 testpoint-extractor 读取  |
| testpoint-extractor     | 测试点 JSON      | 被 testpoint-reviewer 读取   |
| testpoint-reviewer      | 评审后测试点     | 被 testcase-exporter 读取   |
| testcase-exporter       | Excel / XMind    | 最终交付物                   |

---

## 8. 输出文件规范

### 8.1 文件命名

```markdown
# Excel 文件
{项目名称}_测试用例_{范围}_{日期}.xlsx
例：国航电子商务项目二期_测试用例_第3章_新品牌运价业务_20260411.xlsx
    国航电子商务项目二期_测试用例_全部章节_20260411.xlsx

# XMind 文件
{项目名称}_测试用例_{范围}_{日期}.xmind
例：国航电子商务项目二期_测试用例_第3章_新品牌运价业务_20260411.xmind
    国航电子商务项目二期_测试用例_全部章节_20260411.xmind
```

### 8.2 导出格式说明

| 格式 | 输出目录 | 说明 |
|------|----------|------|
| excel | 06_testcase_export/ | Excel 表格格式 |
| xmind | 07_xmind_export/ | XMind 思维导图格式 |
| both | 两个目录都输出 | 同时生成两种格式 |

---

## 9. 使用示例

### 示例1：导出为 Excel

```
请导出测试用例（Excel格式）：
- 测试点：output/电子商务项目二期/04_testpoints/testpoints_summary.json
- 格式：excel
- 输出目录：output/电子商务项目二期/
```

### 示例2：导出为 XMind

```
请导出测试用例（XMind格式）：
- 测试点：output/电子商务项目二期/05_testpoint_review/
- 格式：xmind
- 输出目录：output/电子商务项目二期/
```

### 示例3：同时导出两种格式

```
请导出测试用例：
- 测试点：output/电子商务项目二期/05_testpoint_review/testpoints_summary.json
- 格式：both
- 输出目录：output/电子商务项目二期/
```

### 智能体响应

```
收到任务，开始导出测试用例。

📄 输入信息
- 测试点文件：05_testpoint_review/testpoints_summary.json
- 导出格式：both（Excel + XMind）
- 总用例数：350 个
- 章节数：7 个

🔍 正在读取测试点数据...
[✅] 读取完成

📋 正在导出 Excel...
[⏳] 创建封面 Sheet...
[⏳] 创建第3章 Sheet (80个用例)...
[⏳] 创建第4章 Sheet (41个用例)...
...
[⏳] 创建汇总统计 Sheet...
[✅] Excel 导出完成

📋 正在导出 XMind...
[⏳] 构建思维导图结构...
[⏳] 第3章 新品牌运价业务 (80个测试点)
[⏳] 第4章 常旅客直减功能 (41个测试点)
...
[✅] XMind 导出完成

📁 输出文件
┌─────────────────────────────────────────────────────┐
│  06_testcase_export/ (Excel)                         │
│  ├── 国航电子商务项目二期_测试用例_第3章_20260411.xlsx │
│  ├── 国航电子商务项目二期_测试用例_第4章_20260411.xlsx │
│  └── 国航电子商务项目二期_测试用例_全部章节_20260411.xlsx
│                                                      │
│  07_xmind_export/ (XMind)                            │
│  ├── 国航电子商务项目二期_测试用例_第3章_20260411.xmind│
│  ├── 国航电子商务项目二期_测试用例_第4章_20260411.xmind│
│  └── 国航电子商务项目二期_测试用例_全部章节_20260411.xmind│
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│  导出完成                                            │
├─────────────────────────────────────────────────────┤
│  Excel 文件：3 个（共 350 个用例）                   │
│  XMind 文件：3 个（共 350 个用例）                  │
│  评审状态：已评审                                    │
└─────────────────────────────────────────────────────┘
```

---

## 10. 错误处理

| 错误情况            | 处理方式                                       |
| ------------------ | -------------------------------------------- |
| 测试点JSON不存在    | 报错退出，提示先执行 testpoint-extractor       |
| JSON格式异常        | 跳过异常记录，记录日志，继续处理其他             |
| 章节数据为空        | 跳过该章节，记录警告                           |
| Excel写入失败       | 报错退出，提示检查输出目录权限                   |
| XMind打包失败       | 报错退出，提示检查输出目录权限                   |
| 图片文件缺失        | 继续处理，在notes中标注"图片缺失"               |
| 数据为空            | 报错退出，提示测试点数据为空                     |
| 字段缺失            | 使用空值填充，继续处理                          |
| 格式参数无效        | 报错退出，提示有效值为 excel/xmind/both        |

---

## 11. 验收标准

### 11.1 Excel 验收清单

```
□ Excel文件成功创建（.xlsx）
□ 封面Sheet包含项目信息
□ 每个章节有独立的Sheet
□ 表头行格式正确（蓝色背景白色字体）
□ 数据行交替颜色
□ 所有必填列都有数据
□ 数组字段正确转换为文本
□ 汇总统计Sheet包含统计信息
□ 文件可正常打开
```

### 11.2 XMind 验收清单

```
□ XMind文件成功创建（.xmind 实质是 ZIP）
□ content.json 格式正确，可被 XMind 打开
□ 根节点显示项目名称
□ 章节节点层级正确
□ 需求节点包含正确的测试点
□ 测试点节点显示优先级标签 [P0]/[P1]/[P2]/[P3]
□ 测试点详情（前置条件/步骤/结果）正确展开
□ 统计概要节点显示汇总信息
□ 文件可被 XMind 8/XMind Zen 正常打开
□ 无文件损坏或内容缺失
```

### 11.3 通用验收清单

```
□ 输出目录结构正确
□ 文件命名符合规范
□ 章节覆盖完整
□ 统计数据准确
□ 无编码乱码问题
```

---

## 12. 代码实现参考

### 12.1 XMind 生成实现（使用 xmind_script.py）

**重要**：请直接使用 `xmind_script.py` 中的 `Topic`、`Sheet` 类和 `create_zen()`/`create_legacy()` 函数。

#### 核心数据模型

```python
from xmind_script import Topic, Sheet, create_zen, create_legacy
```

#### XMind 导出核心代码

```python
import sys
sys.path.insert(0, 'g:/AI/上课代码/AI2604/Claude_Code_Skills_01')
from xmind_script import Topic, Sheet, create_zen, create_legacy
from pathlib import Path

def export_testpoints_to_xmind(all_testpoints, project_name, output_path, xmind_format='zen'):
    """将测试点导出为 XMind 文件"""

    # 创建 Sheet
    sheet = Sheet(title=f"{project_name}测试用例")
    sheet.root_topic = Topic(title=f"{project_name}测试用例")

    # ========== 统计概要节点 ==========
    total = sum(len(d['testpoints']) for d in all_testpoints.values())
    type_count = {}
    priority_count = {}
    for data in all_testpoints.values():
        for tp in data['testpoints']:
            tt = tp['test_type'].split(' ')[0]
            type_count[tt] = type_count.get(tt, 0) + 1
            p = tp['priority']
            priority_count[p] = priority_count.get(p, 0) + 1

    summary = Topic(title=f"📊 测试点统计: 共 {total} 个")
    summary.children.append(Topic(title="按类型: " + " | ".join([f"{k}:{v}" for k, v in type_count.items()])))
    summary.children.append(Topic(title="按优先级: " + " | ".join([f"{k}:{v}" for k, v in sorted(priority_count.items())])))
    sheet.root_topic.children.append(summary)

    # ========== 章节节点 ==========
    for chapter_num in sorted(all_testpoints.keys()):
        chapter_data = all_testpoints[chapter_num]
        tp_count = len(chapter_data['testpoints'])

        chapter_topic = Topic(title=f"第{chapter_num}章 {chapter_data['title']} ({tp_count}个)")

        # 按需求分组
        req_groups = {}
        for tp in chapter_data['testpoints']:
            req_id = tp['requirement_id']
            if req_id not in req_groups:
                req_groups[req_id] = []
            req_groups[req_id].append(tp)

        for req_id in sorted(req_groups.keys()):
            req_tps = req_groups[req_id]
            req_topic = Topic(title=f"{req_id} {req_tps[0]['requirement_name']}")

            for tp in req_tps:
                tp_topic = _create_testpoint_topic(tp)
                req_topic.children.append(tp_topic)

            chapter_topic.children.append(req_topic)

        req_count = len(req_groups)
        chapter_topic.children.append(Topic(title=f"📋 小计: {req_count}个需求, {tp_count}个测试点"))
        sheet.root_topic.children.append(chapter_topic)

    # 创建 XMind 文件
    sheets = [sheet]
    if xmind_format == 'legacy':
        create_legacy(sheets, str(output_path))
    else:
        create_zen(sheets, str(output_path))

    return output_path


def _create_testpoint_topic(tp):
    """创建测试点节点（带详情子节点）"""
    priority_emoji = {"P0": "🔴", "P1": "🟠", "P2": "🟡", "P3": "⚪"}.get(tp['priority'], "")
    test_type_short = tp['test_type'].split(' ')[0] if ' ' in tp['test_type'] else tp['test_type']

    tp_topic = Topic(title=f"{priority_emoji} [{tp['priority']}] {tp['testpoint_name']} ({test_type_short})")

    if tp.get('preconditions'):
        pre = Topic(title="📋 前置条件")
        pre.notes = "\n".join([f"• {p}" for p in tp['preconditions']])
        tp_topic.children.append(pre)

    if tp.get('test_steps'):
        steps = tp['test_steps']
        if isinstance(steps, list):
            steps_text = "\n".join([f"{i+1}. {s}" for i, s in enumerate(steps)])
        else:
            steps_text = str(steps)
        step = Topic(title="📝 测试步骤")
        step.notes = steps_text
        tp_topic.children.append(step)

    if tp.get('expected_results'):
        results = Topic(title="✅ 预期结果")
        results.notes = "\n".join([f"• {r}" for r in tp['expected_results']])
        tp_topic.children.append(results)

    if tp.get('test_data'):
        data = Topic(title="📊 测试数据")
        data.notes = "\n".join([f"• {d}" for d in tp['test_data']])
        tp_topic.children.append(data)

    return tp_topic
```

### 12.2 调用方式

```python
from pathlib import Path

# 加载测试点数据
testpoints_dir = Path("output/电子商务项目二期/04_testpoints")
all_testpoints = load_testpoints_from_dir(testpoints_dir)

# 导出为 Zen 格式（JSON，推荐）
output_zen = Path("output/电子商务项目二期/07_xmind_export/测试用例_zen.xmind")
export_testpoints_to_xmind(all_testpoints, "国航电子商务项目二期", output_zen, xmind_format='zen')

# 导出为 Legacy 格式（XML，兼容 XMind 8）
output_legacy = Path("output/电子商务项目二期/07_xmind_export/测试用例_legacy.xmind")
export_testpoints_to_xmind(all_testpoints, "国航电子商务项目二期", output_legacy, xmind_format='legacy')
```

### 12.3 层级结构对照

```
测试点 JSON              Topic 树结构              XMind 显示
─────────────────────────────────────────────────────────────────
项目名称                   Sheet.root_topic         根节点
  │
  ├── 统计概要 ──────────► summary Topic            统计信息
  │
  └── 第3章 ────────────► chapter Topic            一级子节点
          │
          ├── REQ-CH3-001 ─► req Topic              二级子节点
          │       │
          │       └── TP ───► tp Topic              三级子节点
          │               ├── [前置条件] ─────────► 四级子节点(notes)
          │               ├── [测试步骤] ─────────► 四级子节点(notes)
          │               └── [预期结果] ─────────► 四级子节点(notes)
          │
          └── 小计 ────────► summary Topic           章节统计
```

### 12.4 XMind 文件格式说明

| 格式 | 文件类型 | 适用版本 | 文件内容 |
|------|---------|---------|---------|
| `zen` | JSON | XMind Zen / 2020+ | content.json + metadata.json |
| `legacy` | XML | XMind 8 | content.xml + META-INF/manifest.xml |

**推荐使用 `zen` 格式**，兼容性更好。
