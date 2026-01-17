# PROC_META_001 元审计规则质量审计

## 程序标识
- **程序ID**: PROC_META_001
- **程序名称**: 元审计规则质量审计
- **适用规则**: META_001~META_015（所有元审计规则）
- **适用范围**: 审计规则文件（tests/fixtures/rule/*.md）

## 审计目标
- **主要目标**: 确保审计规则符合元审计规则的质量要求
- **次要目标**: 生成规则质量评分和改进建议

## 执行步骤

### 准备阶段
1. **收集目标规则文件**
   ```bash
   find tests/fixtures/rule -name "*.md" -not -name "meta.md"
   ```

2. **加载元审计规则**
   - 读取 `tests/fixtures/rule/meta.md`
   - 解析所有元审计规则

3. **准备检查清单**
   - 提取元审计规则中的检查清单
   - 准备验证脚本

### 执行阶段
1. **完整性检查**
   ```python
   # 检查项：
   - 规则名称、定义、指标、检测方法
   - 规则标注检测方式（自动/半自动/人工）
   - 阈值或判断标准明确无歧义
   ```

2. **有效性检查**
   ```python
   # 检查项：
   - 规则对应真实问题或风险
   - 规则结果可验证
   - 规则覆盖度合理
   ```

3. **可理解性检查**
   ```python
   # 检查项：
   - 规则名称简洁明确
   - 规则描述清晰易懂
   - 提供正反代码示例
   ```

4. **实用性检查**
   ```python
   # 检查项：
   - 误报率在可接受范围
   - 修复成本合理
   - 标注优先级（Critical/Major/Minor）
   ```

5. **一致性检查**
   ```python
   # 检查项：
   - 遵循行业标准
   - 通过团队评审
   - 跨项目规则统一
   ```

### 分析阶段
1. **计算通过率**
   ```python
   通过率 = (通过项数 / 总检查项数) × 100%
   ```

2. **严重程度分级**
   - 通过率 < 50%：CRITICAL（必须重构）
   - 通过率 50%-80%：MAJOR（需要改进）
   - 通过率 80%-90%：MINOR（建议优化）
   - 通过率 ≥ 90%：PASS（符合要求）

3. **生成审计发现**
   - 记录未通过项
   - 生成整改建议

### 报告阶段
1. **整理审计发现**
   - 按严重程度分组
   - 按规则维度分类

2. **生成审计报告**
   - 审计概览
   - 发现汇总
   - 改进建议

3. **归档证据**
   - 检查清单结果
   - 评分详情

## 输入输出
- **输入**:
  - 元审计规则：tests/fixtures/rule/meta.md
  - 目标规则文件：tests/fixtures/rule/*.md（除 meta.md）
- **输出**:
  - 审计发现：tests/fixtures/issue/META_001_rule_quality.md
  - 审计报告：tests/fixtures/report/META_001_report.md
  - 检查清单：tests/fixtures/evidence/meta_checklist_{timestamp}.json

## 工具依赖
- **验证脚本**: src/python_sdk/meta_audit_validator.py
- **环境**: Python 3.8+
- **依赖库**: markdown, pyyaml, json

---

## 检查清单详情

### 完整性检查（3 项）
- [ ] 规则包含名称、定义、指标、检测方法
- [ ] 规则标注检测方式（自动/半自动/人工）
- [ ] 阈值或判断标准明确无歧义

### 有效性检查（3 项）
- [ ] 规则对应真实问题或风险
- [ ] 规则结果可验证
- [ ] 规则覆盖度合理

### 可理解性检查（3 项）
- [ ] 规则名称简洁明确
- [ ] 规则描述清晰易懂
- [ ] 提供正反代码示例

### 实用性检查（3 项）
- [ ] 误报率在可接受范围
- [ ] 修复成本合理
- [ ] 标注优先级（Critical/Major/Minor）

### 一致性检查（3 项）
- [ ] 遵循行业标准
- [ ] 通过团队评审
- [ ] 跨项目规则统一

---

## 执行命令

```bash
# 执行元审计
python src/python_sdk/meta_audit_validator.py \
    --rule-dir tests/fixtures/rule/ \
    --output-dir tests/fixtures/evidence/

# 生成审计报告
python src/python_sdk/generate_meta_audit_report.py \
    --checklist tests/fixtures/evidence/meta_checklist_*.json \
    --output tests/fixtures/report/META_001_report.md

# 查看审计结果
cat tests/fixtures/report/META_001_report.md
```

---

## 评分标准

| 通过率 | 严重程度 | 处理建议 |
|--------|---------|---------|
| ≥ 90% | PASS | 规则质量优秀，持续维护 |
| 80%-89% | MINOR | 规则质量良好，建议优化 |
| 50%-79% | MAJOR | 规则质量一般，需要改进 |
| < 50% | CRITICAL | 规则质量不合格，必须重构 |

---

## 示例输出

```json
{
  "audit_id": "META_001_20250116",
  "rule_file": "tests/fixtures/rule/python_code_quality.md",
  "timestamp": "2025-01-16T15:30:00Z",
  "checks": {
    "completeness": {
      "total": 3,
      "passed": 2,
      "failed": 1,
      "items": [
        {"name": "规则包含名称、定义、指标、检测方法", "status": "passed"},
        {"name": "规则标注检测方式", "status": "failed", "reason": "未标注检测方式"},
        {"name": "阈值明确", "status": "passed"}
      ]
    },
    "validity": {
      "total": 3,
      "passed": 3,
      "failed": 0,
      "items": [...]
    },
    "understandability": {
      "total": 3,
      "passed": 3,
      "failed": 0,
      "items": [...]
    },
    "practicality": {
      "total": 3,
      "passed": 1,
      "failed": 2,
      "items": [...]
    },
    "consistency": {
      "total": 3,
      "passed": 0,
      "failed": 3,
      "items": [...]
    }
  },
  "summary": {
    "total": 15,
    "passed": 9,
    "failed": 6,
    "pass_rate": 60,
    "severity": "MAJOR"
  }
}
```
