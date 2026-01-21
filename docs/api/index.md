# API 文档

本文档提供项目的完整 API 参考。如果可能，本 API 文档会自动从代码注释生成。

## 概述

项目提供以下主要模块的 API：

1. **核心模块** - 基础功能和工具
2. **数据处理模块** - 数据加载、转换和清洗
3. **分析模块** - 数据分析和模型训练
4. **工具模块** - 辅助工具和实用函数

## 自动生成说明

如果项目使用自动文档生成工具（如 Sphinx、pydoc、Doxygen），则此部分内容应由工具自动生成。

### 设置自动文档生成

#### 使用 Sphinx (Python)

1. 安装 Sphinx：

```bash
pip install sphinx sphinx-rtd-theme
```

2. 初始化文档：

```bash
sphinx-quickstart docs/api/source
```

3. 配置 `conf.py`：

```python
import os
import sys
sys.path.insert(0, os.path.abspath('../../..'))

extensions = [
    'sphinx.ext.autodoc',
    'sphinx.ext.napoleon',
    'sphinx.ext.viewcode',
    'sphinx.ext.githubpages'
]

html_theme = 'sphinx_rtd_theme'
```

4. 生成文档：

```bash
cd docs/api
sphinx-apidoc -o source/ ../../your_project
make html
```

#### 使用 TypeDoc (TypeScript/JavaScript)

```bash
npm install typedoc --save-dev
npx typedoc --out docs/api --entryPoints src/index.ts
```

## 核心 API 参考

### 项目类 (Project)

**类定义**：

```python
class Project:
    """项目主类，提供核心功能"""
    
    def __init__(self, config_path: str = None, log_level: str = "INFO"):
        """
        初始化项目
        
        参数:
            config_path: 配置文件路径
            log_level: 日志级别 (DEBUG, INFO, WARNING, ERROR)
        """
        pass
    
    def load_config(self, config_path: str = None) -> dict:
        """
        加载配置文件
        
        参数:
            config_path: 配置文件路径，如果为None则使用初始化时的路径
        
        返回:
            配置字典
        """
        pass
    
    def run(self, input_data: Any = None) -> dict:
        """
        运行项目
        
        参数:
            input_data: 输入数据
        
        返回:
            运行结果
        """
        pass
```

### 数据处理模块

#### DataLoader 类

```python
class DataLoader:
    """数据加载器，支持多种格式"""
    
    def load_csv(self, filepath: str, **kwargs) -> pd.DataFrame:
        """
        加载CSV文件
        
        参数:
            filepath: CSV文件路径
            **kwargs: 传递给pandas.read_csv的额外参数
        
        返回:
            pandas DataFrame
        """
        pass
    
    def load_json(self, filepath: str, **kwargs) -> dict:
        """
        加载JSON文件
        
        参数:
            filepath: JSON文件路径
            **kwargs: 传递给json.load的额外参数
        
        返回:
            JSON数据字典
        """
        pass
    
    def load_from_database(self, query: str, connection: Any = None) -> pd.DataFrame:
        """
        从数据库加载数据
        
        参数:
            query: SQL查询语句
            connection: 数据库连接对象
        
        返回:
            pandas DataFrame
        """
        pass
```

#### DataTransformer 类

```python
class DataTransformer:
    """数据转换器，提供数据清洗和转换功能"""
    
    def clean_data(self, df: pd.DataFrame, options: dict = None) -> pd.DataFrame:
        """
        清洗数据
        
        参数:
            df: 输入DataFrame
            options: 清洗选项
        
        返回:
            清洗后的DataFrame
        """
        pass
    
    def normalize(self, df: pd.DataFrame, columns: List[str] = None) -> pd.DataFrame:
        """
        数据标准化
        
        参数:
            df: 输入DataFrame
            columns: 要标准化的列名列表，如果为None则标准化所有数值列
        
        返回:
            标准化后的DataFrame
        """
        pass
    
    def encode_categorical(self, df: pd.DataFrame, columns: List[str] = None) -> pd.DataFrame:
        """
        分类变量编码
        
        参数:
            df: 输入DataFrame
            columns: 要编码的列名列表，如果为None则编码所有分类列
        
        返回:
            编码后的DataFrame
        """
        pass
```

### 分析模块

#### Analyzer 类

```python
class Analyzer:
    """数据分析器，提供统计分析功能"""
    
    def describe(self, df: pd.DataFrame) -> dict:
        """
        数据描述统计
        
        参数:
            df: 输入DataFrame
        
        返回:
            描述统计字典
        """
        pass
    
    def correlation_analysis(self, df: pd.DataFrame, method: str = "pearson") -> pd.DataFrame:
        """
        相关性分析
        
        参数:
            df: 输入DataFrame
            method: 相关性计算方法 ("pearson", "spearman", "kendall")
        
        返回:
            相关性矩阵DataFrame
        """
        pass
    
    def hypothesis_test(self, df: pd.DataFrame, test_type: str, **kwargs) -> dict:
        """
        假设检验
        
        参数:
            df: 输入DataFrame
            test_type: 检验类型 ("t-test", "chi-square", "anova")
            **kwargs: 检验特定参数
        
        返回:
            检验结果字典
        """
        pass
```

#### ModelTrainer 类

```python
class ModelTrainer:
    """模型训练器，提供机器学习模型训练功能"""
    
    def train(self, X, y, model_type: str = "random_forest", **kwargs) -> Any:
        """
        训练模型
        
        参数:
            X: 特征数据
            y: 目标数据
            model_type: 模型类型
            **kwargs: 模型特定参数
        
        返回:
            训练好的模型
        """
        pass
    
    def evaluate(self, model, X_test, y_test, metrics: List[str] = None) -> dict:
        """
        评估模型
        
        参数:
            model: 训练好的模型
            X_test: 测试特征数据
            y_test: 测试目标数据
            metrics: 评估指标列表
        
        返回:
            评估结果字典
        """
        pass
    
    def cross_validate(self, X, y, model_type: str = "random_forest", cv: int = 5, **kwargs) -> dict:
        """
        交叉验证
        
        参数:
            X: 特征数据
            y: 目标数据
            model_type: 模型类型
            cv: 交叉验证折数
            **kwargs: 模型特定参数
        
        返回:
            交叉验证结果字典
        """
        pass
```

### 工具模块

#### Logger 类

```python
class Logger:
    """日志记录器，提供灵活的日志功能"""
    
    def __init__(self, name: str = None, level: str = "INFO"):
        """
        初始化日志记录器
        
        参数:
            name: 日志记录器名称
            level: 日志级别
        """
        pass
    
    def debug(self, message: str, **kwargs):
        """
        记录调试信息
        
        参数:
            message: 日志消息
            **kwargs: 额外参数
        """
        pass
    
    def info(self, message: str, **kwargs):
        """
        记录一般信息
        
        参数:
            message: 日志消息
            **kwargs: 额外参数
        """
        pass
    
    def warning(self, message: str, **kwargs):
        """
        记录警告信息
        
        参数:
            message: 日志消息
            **kwargs: 额外参数
        """
        pass
    
    def error(self, message: str, **kwargs):
        """
        记录错误信息
        
        参数:
            message: 日志消息
            **kwargs: 额外参数
        """
        pass
```

#### ConfigManager 类

```python
class ConfigManager:
    """配置管理器，提供配置加载和管理功能"""
    
    def __init__(self, config_path: str = None):
        """
        初始化配置管理器
        
        参数:
            config_path: 配置文件路径
        """
        pass
    
    def load(self, config_path: str = None) -> dict:
        """
        加载配置
        
        参数:
            config_path: 配置文件路径
        
        返回:
            配置字典
        """
        pass
    
    def save(self, config: dict, config_path: str = None):
        """
        保存配置
        
        参数:
            config: 配置字典
            config_path: 配置文件路径
        """
        pass
    
    def get(self, key: str, default: Any = None) -> Any:
        """
        获取配置值
        
        参数:
            key: 配置键
            default: 默认值
        
        返回:
            配置值
        """
        pass
    
    def set(self, key: str, value: Any):
        """
        设置配置值
        
        参数:
            key: 配置键
            value: 配置值
        """
        pass
```

## 使用示例

### 基本使用

```python
from your_project import Project
from your_project.data_loader import DataLoader
from your_project.analyzer import Analyzer

# 初始化项目
project = Project(config_path="config.yaml")

# 加载配置
config = project.load_config()

# 加载数据
data_loader = DataLoader()
df = data_loader.load_csv("data.csv")

# 分析数据
analyzer = Analyzer()
stats = analyzer.describe(df)
print(stats)
```

### 高级使用

```python
from your_project.model_trainer import ModelTrainer
from your_project.data_transformer import DataTransformer
from sklearn.model_selection import train_test_split

# 准备数据
transformer = DataTransformer()
df_clean = transformer.clean_data(df)
X = df_clean.drop("target", axis=1)
y = df_clean["target"]

# 分割数据
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# 训练模型
trainer = ModelTrainer()
model = trainer.train(X_train, y_train, model_type="random_forest")

# 评估模型
metrics = trainer.evaluate(model, X_test, y_test)
print(f"模型性能: {metrics}")
```

## API 版本

当前 API 版本：**v1.0.0**

### 版本历史

- **v1.0.0** (当前): 初始版本，包含核心功能
- **v0.9.0**: 预览版，包含基本数据加载和分析功能

## 贡献指南

如果您想为 API 文档做出贡献：

1. 确保代码有完整的文档字符串
2. 遵循项目的代码风格指南
3. 使用类型注解提高文档可读性
4. 为公共 API 添加使用示例

## 更多资源

- [快速开始](../getting-started.md) - 安装和基本使用
- [教程](../tutorials/tutorial-quick.md) - 详细使用指南
- [示例](../examples/example1.md) - 实际应用示例
