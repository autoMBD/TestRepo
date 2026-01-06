# MODEL_A 文档 (Simulink 模型)

本文档描述 MODEL_A Simulink 模型的设计、功能和使用方法。

## 模型概述

### 基本信息
- **模型名称**: MODEL_A
- **版本**: 1.0.0
- **创建日期**: 2024-01-01
- **最后修改**: 2024-06-01
- **作者**: 项目团队
- **依赖**: Simulink R2022a 或更高版本

### 功能描述
MODEL_A 是一个用于 [此处描述模型主要功能，例如：控制系统仿真、信号处理、电力系统分析等] 的 Simulink 模型。

### 应用场景
- 场景 1: [具体应用场景 1]
- 场景 2: [具体应用场景 2]
- 场景 3: [具体应用场景 3]

## 模型结构

### 顶层结构图
```
┌─────────────────────────────────────────────────────┐
│                    MODEL_A (顶层)                    │
├─────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐         │
│  │ 输入模块 │  │ 处理模块 │  │ 输出模块 │         │
│  └──────────┘  └──────────┘  └──────────┘         │
│         │             │             │              │
│         └─────┬───────┴───────┬─────┘              │
│               │               │                    │
│         ┌─────▼─────┐   ┌─────▼─────┐              │
│         │  控制逻辑  │   │  监控模块  │              │
│         └───────────┘   └───────────┘              │
└─────────────────────────────────────────────────────┘
```

### 子系统说明

#### 1. 输入模块 (Input_Subsystem)
**功能**: 负责接收和处理输入信号

**参数**:
- `SampleTime`: 采样时间 (默认: 0.01s)
- `InputRange`: 输入范围 (默认: [-10, 10])
- `NoiseLevel`: 噪声水平 (默认: 0.01)

**接口**:
```
输入端口:
  1. In1: 主输入信号
  2. In2: 参考信号
  3. In3: 配置参数

输出端口:
  1. Out1: 处理后的输入信号
  2. Out2: 输入状态标志
```

#### 2. 处理模块 (Processing_Subsystem)
**功能**: 执行核心算法和处理逻辑

**包含组件**:
- **控制器**: PID 控制器模块
- **滤波器**: 低通滤波器 (截止频率: 10Hz)
- **转换器**: 信号转换和缩放

**算法描述**:
```matlab
% 核心处理算法
function output = processAlgorithm(input, params)
    % 步骤 1: 信号预处理
    processed = preprocess(input, params.preprocess);
    
    % 步骤 2: 核心计算
    result = coreCalculation(processed, params.core);
    
    % 步骤 3: 后处理
    output = postprocess(result, params.postprocess);
end
```

#### 3. 输出模块 (Output_Subsystem)
**功能**: 生成输出信号和状态信息

**输出类型**:
1. **连续输出**: 主要控制信号
2. **离散输出**: 状态标志和事件
3. **监控输出**: 性能指标和诊断信息

#### 4. 控制逻辑 (Control_Logic)
**功能**: 实现状态机和模式切换

**状态定义**:
```matlab
states = {
    'INIT',      % 初始化状态
    'STANDBY',   % 待机状态
    'RUNNING',   % 运行状态
    'FAULT',     % 故障状态
    'SHUTDOWN'   % 关机状态
};
```

#### 5. 监控模块 (Monitoring_Subsystem)
**功能**: 实时监控和诊断

**监控指标**:
- 系统稳定性指标
- 性能指标 (响应时间、超调量等)
- 故障检测和诊断

## 参数配置

### 主要参数表

| 参数名 | 描述 | 默认值 | 单位 | 范围 |
|--------|------|--------|------|------|
| `Kp` | 比例增益 | 1.0 | - | [0.1, 10] |
| `Ki` | 积分增益 | 0.1 | - | [0, 5] |
| `Kd` | 微分增益 | 0.01 | - | [0, 2] |
| `Ts` | 采样时间 | 0.01 | s | [0.001, 1] |
| `Fc` | 截止频率 | 10 | Hz | [1, 100] |
| `MaxOutput` | 最大输出 | 100 | % | [0, 100] |
| `MinOutput` | 最小输出 | 0 | % | [0, 100] |

### 配置示例
```matlab
% MATLAB 配置脚本
model_params = struct();
model_params.controller.Kp = 1.5;
model_params.controller.Ki = 0.2;
model_params.controller.Kd = 0.05;
model_params.sampling.Ts = 0.005;
model_params.filter.Fc = 15;
model_params.limits.MaxOutput = 90;
model_params.limits.MinOutput = 10;

% 应用配置到模型
set_param('MODEL_A/Controller', 'Kp', num2str(model_params.controller.Kp));
set_param('MODEL_A/Controller', 'Ki', num2str(model_params.controller.Ki));
% ... 其他参数设置
```

## 使用指南

### 1. 打开和加载模型
```matlab
% 方法 1: 使用 open_system
open_system('MODEL_A.slx');

% 方法 2: 使用 load_system 然后打开
load_system('MODEL_A.slx');
open_system('MODEL_A');
```

### 2. 运行仿真
```matlab
% 基本仿真
simOut = sim('MODEL_A.slx', 'StopTime', '10');

% 带参数的仿真
simOut = sim('MODEL_A.slx', ...
    'StopTime', '20', ...
    'FixedStep', '0.01', ...
    'Solver', 'ode4');

% 批量仿真 (参数扫描)
paramValues = 0.1:0.1:1.0;
results = cell(length(paramValues), 1);
for i = 1:length(paramValues)
    set_param('MODEL_A/Controller', 'Kp', num2str(paramValues(i)));
    simOut = sim('MODEL_A.slx', 'StopTime', '10');
    results{i} = simOut;
end
```

### 3. 数据记录和分析
```matlab
% 配置数据记录
set_param('MODEL_A', 'SaveOutput', 'on');
set_param('MODEL_A', 'OutputSaveName', 'yout');
set_param('MODEL_A', 'SaveTime', 'on');
set_param('MODEL_A', 'TimeSaveName', 'tout');

% 运行仿真
simOut = sim('MODEL_A.slx');

% 提取和分析数据
time = simOut.tout;
output = simOut.yout;

% 绘制结果
figure;
subplot(2,1,1);
plot(time, output(:,1));
title('输出信号 1');
xlabel('时间 (s)');
ylabel('幅值');

subplot(2,1,2);
plot(time, output(:,2));
title('输出信号 2');
xlabel('时间 (s)');
ylabel('幅值');
```

## 模型验证

### 测试用例

#### 测试 1: 阶跃响应测试
**目的**: 验证系统对阶跃输入的响应特性

**测试步骤**:
1. 设置输入为阶跃信号 (幅值: 1, 时间: 1s)
2. 运行仿真 10 秒
3. 记录和评估响应特性

**验收标准**:
- 上升时间 < 2 秒
- 超调量 < 5%
- 稳态误差 < 1%

#### 测试 2: 频率响应测试
**目的**: 验证系统频率特性

**测试步骤**:
1. 使用正弦扫频输入 (频率: 0.1-10Hz)
2. 记录输出幅值和相位
3. 绘制 Bode 图

**验收标准**:
- 带宽 > 5Hz
- 相位裕度 > 45°

#### 测试 3: 鲁棒性测试
**目的**: 验证系统对参数变化的鲁棒性

**测试步骤**:
1. 在参数范围内随机变化关键参数
2. 运行蒙特卡洛仿真
3. 统计性能指标

**验收标准**:
- 95% 的仿真满足性能要求

## 性能指标

### 静态性能
- **稳态误差**: < 1%
- **线性度**: > 99%
- **重复性**: > 99.5%

### 动态性能
- **上升时间**: 1.5 ± 0.3 秒
- **调节时间**: 3.0 ± 0.5 秒
- **超调量**: < 5%

### 计算性能
- **仿真步长**: 0.01 秒
- **实时因子**: < 0.1 (远快于实时)
- **内存使用**: < 100 MB

## 故障排除

### 常见问题

#### 问题 1: 仿真发散
**症状**: 输出值无限增大或变为 NaN

**可能原因**:
1. 控制器参数不合适 (特别是积分项过大)
2. 代数环问题
3. 采样时间设置不当

**解决方案**:
1. 检查并调整控制器参数
2. 使用 `Algebraic Loop Solver`
3. 减小采样时间或使用变步长求解器

#### 问题 2: 仿真速度慢
**症状**: 仿真运行时间过长

**可能原因**:
1. 过小的固定步长
2. 复杂的模型结构
3. 过多的数据记录

**解决方案**:
1. 适当增大固定步长或使用变步长求解器
2. 简化模型结构，使用封装子系统
3. 减少数据记录点或使用触发记录

#### 问题 3: 模型编译错误
**症状**: 编译时出现错误

**可能原因**:
1. 模块参数设置错误
2. 数据类型不匹配
3. 缺少必需的依赖项

**解决方案**:
1. 检查错误信息中提到的模块和参数
2. 使用 `Model Advisor` 检查模型
3. 确保所有依赖的库和工具箱已安装

### 调试技巧
1. **使用信号记录**: 在关键点添加信号记录
2. **断点调试**: 使用 Simulink 调试器设置断点
3. **模型覆盖度**: 使用 `Simulink Coverage` 分析测试覆盖度
4. **性能分析**: 使用 `Simulink Profiler` 分析仿真性能

## 代码生成

### 生成 C 代码
```matlab
% 配置代码生成
rtwconfig = get_param('MODEL_A', 'RTWGenSettings');
rtwconfig.TargetLang = 'C';
rtwconfig.GenCodeOnly = 'off';
rtwconfig.GenerateReport = 'on';

% 生成代码
slbuild('MODEL_A');

% 查看生成报告
rtwview('MODEL_A');
```

### 生成的代码结构
```
MODEL_A_ert_rtw/
├── MODEL_A.c          # 主模型代码
├── MODEL_A.h          # 模型头文件
├── MODEL_A_private.h  # 私有头文件
├── MODEL_A_types.h    # 类型定义
├── rtmodel.h          # 实时模型头文件
└── html/              # 代码生成报告
```

### 集成到目标系统
1. **手动集成**: 将生成的代码文件添加到目标项目
2. **自动集成**: 使用 Simulink Coder 支持包
3. **验证**: 使用 PIL (Processor-in-the-Loop) 测试

## 版本历史

### v1.0.0 (2024-06-01)
- 初始发布版本
- 包含基本控制功能
- 支持代码生成

### v0.9.0 (2024-05-15)
- Beta 测试版本
- 添加监控和诊断功能
- 改进参数配置接口

### v0.8.0 (2024-04-30)
- Alpha 测试版本
- 基本模型结构完成
- 初步验证通过

## 相关文档

- [MODEL_B 文档](./MODEL_B.md) - 相关模型文档
- [Simulink 基础教程](../tutorials/tutorial-quick.md) - Simulink 使用教程
- [示例数据](../examples/example-data/) - 示例模型和脚本
- [API 文档](../api/index.md) - 项目 API 参考
