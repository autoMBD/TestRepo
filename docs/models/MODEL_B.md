# MODEL_B 文档 (Simulink 模型)

本文档描述 MODEL_B Simulink 模型的设计、功能和使用方法。

## 模型概述

### 基本信息
- **模型名称**: MODEL_B
- **版本**: 1.0.0
- **创建日期**: 2024-02-01
- **最后修改**: 2024-06-15
- **作者**: 项目团队
- **依赖**: Simulink R2022a 或更高版本，Control System Toolbox

### 功能描述
MODEL_B 是一个高级控制系统模型，专为 [此处描述模型主要功能，例如：多变量控制、自适应控制、模型预测控制等] 设计。该模型扩展了 MODEL_A 的功能，增加了高级控制算法和更复杂的系统动力学。

### 主要特性
- **多变量控制**: 支持多输入多输出（MIMO）系统
- **自适应机制**: 可根据系统变化自动调整参数
- **故障容错**: 内置故障检测和恢复机制
- **优化控制**: 集成优化算法以实现最优性能

### 应用场景
- 场景 1: 工业过程控制（如化工、制药）
- 场景 2: 机器人运动控制
- 场景 3: 能源管理系统
- 场景 4: 自动驾驶系统

## 模型结构

### 顶层结构图
```
┌─────────────────────────────────────────────────────────┐
│                     MODEL_B (顶层)                       │
├─────────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌────────────┐  ┌──────────┐           │
│  │ 多路输入  │  │ 高级处理器  │  │ 多路输出  │           │
│  └──────────┘  └────────────┘  └──────────┘           │
│         │                │                │            │
│         └────────┬───────┴───────┬────────┘            │
│                  │               │                     │
│           ┌──────▼─────┐ ┌───────▼──────┐              │
│           │ 自适应控制器 │ │ 优化引擎    │              │
│           └────────────┘ └──────────────┘              │
│                  │               │                     │
│           ┌──────▼─────┐ ┌───────▼──────┐              │
│           │ 状态估计器  │ │ 故障诊断模块 │              │
│           └────────────┘ └──────────────┘              │
└─────────────────────────────────────────────────────────┘
```

### 子系统说明

#### 1. 多路输入模块 (MultiInput_Subsystem)
**功能**: 处理多个输入信号并进行同步

**输入通道**:
- **主输入通道** (4个): 主要控制信号
- **辅助输入通道** (2个): 参考和干扰信号
- **配置输入通道** (1个): 实时参数调整

**特性**:
- 信号同步和时钟对齐
- 输入范围自动缩放
- 通道间交叉干扰抑制

#### 2. 高级处理器 (AdvancedProcessor_Subsystem)
**功能**: 执行复杂算法和数据处理

**包含组件**:
- **状态空间控制器**: 基于状态空间模型的控制
- **卡尔曼滤波器**: 状态估计和噪声过滤
- **模型预测控制器**: 有限时域优化控制
- **自适应律**: 在线参数调整

**算法描述**:
```matlab
% 高级处理算法
function [control, state] = advancedAlgorithm(inputs, states, params)
    % 步骤 1: 状态估计
    estimated_states = kalmanFilter(states, inputs, params.kalman);
    
    % 步骤 2: 模型预测控制
    if params.use_mpc
        control = modelPredictiveControl(estimated_states, inputs, params.mpc);
    else
        % 步骤 3: 自适应控制
        control = adaptiveControl(estimated_states, inputs, params.adaptive);
    end
    
    % 步骤 4: 优化调整
    if params.optimization.enabled
        control = optimizeControl(control, estimated_states, params.optimization);
    end
end
```

#### 3. 多路输出模块 (MultiOutput_Subsystem)
**功能**: 生成多通道输出和状态信息

**输出类型**:
1. **控制输出** (4通道): 主要控制信号
2. **状态输出** (6通道): 系统状态信息
3. **诊断输出** (2通道): 故障和性能指标
4. **监控输出** (3通道): 实时监控数据

#### 4. 自适应控制器 (AdaptiveController)
**功能**: 实现自适应控制算法

**自适应机制**:
- **模型参考自适应控制**: 跟踪参考模型行为
- **自校正调节器**: 在线参数估计和调整
- **增益调度**: 基于工作点的参数调整

**参数更新律**:
```matlab
function params = updateParameters(params, error, gradient)
    % 基于梯度的参数更新
    learning_rate = params.learning_rate;
    params.values = params.values - learning_rate * gradient * error;
    
    % 参数约束处理
    params.values = applyConstraints(params.values, params.bounds);
end
```

#### 5. 优化引擎 (OptimizationEngine)
**功能**: 实时优化控制性能

**优化目标**:
- 最小化控制误差
- 最小化控制能量
- 满足约束条件（状态约束、输入约束）

**优化算法**:
- **二次规划**: 用于模型预测控制
- **梯度下降**: 用于在线优化
- **粒子群优化**: 用于全局优化

#### 6. 状态估计器 (StateEstimator)
**功能**: 估计不可测状态变量

**估计方法**:
- 扩展卡尔曼滤波器
- 无迹卡尔曼滤波器
- 粒子滤波器

#### 7. 故障诊断模块 (FaultDiagnosis)
**功能**: 检测和诊断系统故障

**检测方法**:
- 残差分析
- 主成分分析
- 神经网络分类

## 参数配置

### 主要参数表

| 参数名 | 描述 | 默认值 | 单位 | 范围 |
|--------|------|--------|------|------|
| `Q` | 状态权重矩阵 | eye(6) | - | 正定矩阵 |
| `R` | 控制权重矩阵 | eye(4) | - | 正定矩阵 |
| `N` | 预测时域 | 10 | 步 | [5, 50] |
| `M` | 控制时域 | 5 | 步 | [3, 20] |
| `alpha` | 自适应增益 | 0.1 | - | [0.001, 1] |
| `beta` | 遗忘因子 | 0.99 | - | [0.9, 1] |
| `Ts` | 采样时间 | 0.005 | s | [0.001, 0.1] |
| `tol` | 优化容差 | 1e-6 | - | [1e-9, 1e-3] |

### 配置示例
```matlab
% MATLAB 配置脚本
modelB_params = struct();

% 控制器参数
modelB_params.controller.type = 'mpc';  % 'adaptive' 或 'mpc'
modelB_params.controller.Q = diag([1, 1, 0.5, 0.5, 0.1, 0.1]);
modelB_params.controller.R = diag([0.1, 0.1, 0.2, 0.2]);
modelB_params.controller.N = 15;
modelB_params.controller.M = 8;

% 自适应参数
modelB_params.adaptive.alpha = 0.05;
modelB_params.adaptive.beta = 0.995;
modelB_params.adaptive.forgetting_factor = 0.999;

% 优化参数
modelB_params.optimization.solver = 'quadprog';
modelB_params.optimization.max_iter = 100;
modelB_params.optimization.tol = 1e-6;

% 状态估计参数
modelB_params.estimation.type = 'ekf';
modelB_params.estimation.process_noise = 0.01 * eye(6);
modelB_params.estimation.measurement_noise = 0.1 * eye(4);

% 应用配置
configureModelB(modelB_params);
```

## 使用指南

### 1. 模型初始化
```matlab
% 加载模型
load_system('MODEL_B.slx');

% 初始化参数
initParams = getDefaultParameters();
setModelParameters('MODEL_B', initParams);

% 检查模型状态
modelStatus = checkModel('MODEL_B');
if modelStatus.valid
    disp('模型初始化成功');
else
    error('模型初始化失败: %s', modelStatus.message);
end
```

### 2. 运行仿真
```matlab
% 基本仿真配置
simConfig = struct();
simConfig.StopTime = '30';
simConfig.Solver = 'ode4';
simConfig.FixedStep = '0.005';
simConfig.SaveState = 'on';
simConfig.StateSaveName = 'xout';
simConfig.SaveOutput = 'on';
simConfig.OutputSaveName = 'yout';

% 运行仿真
simOut = sim('MODEL_B.slx', simConfig);

% 或者使用简化语法
simOut = sim('MODEL_B.slx', 'StopTime', '30', 'FixedStep', '0.005');
```

### 3. 高级仿真场景
```matlab
% 场景 1: 参数扫描
param_values = linspace(0.1, 2.0, 20);
results = struct();

for i = 1:length(param_values)
    % 更新参数
    set_param('MODEL_B/AdaptiveController', 'alpha', num2str(param_values(i)));
    
    % 运行仿真
    simOut = sim('MODEL_B.slx', 'StopTime', '20');
    
    % 保存结果
    results(i).param_value = param_values(i);
    results(i).performance = calculatePerformance(simOut);
    results(i).data = simOut;
end

% 分析结果
analyzeParameterSweep(results);

% 场景 2: 蒙特卡洛仿真
num_runs = 100;
monte_carlo_results = cell(num_runs, 1);

for run = 1:num_runs
    % 随机扰动参数
    perturbed_params = perturbParameters(getDefaultParameters());
    setModelParameters('MODEL_B', perturbed_params);
    
    % 运行仿真
    simOut = sim('MODEL_B.slx', 'StopTime', '15');
    
    % 记录结果
    monte_carlo_results{run} = simOut;
end

% 统计性能
performance_stats = analyzeMonteCarlo(monte_carlo_results);
```

### 4. 数据分析和可视化
```matlab
% 提取仿真数据
time = simOut.tout;
outputs = simOut.yout;
states = simOut.xout;

% 创建综合可视化
figure('Position', [100, 100, 1200, 800]);

% 子图 1: 输出响应
subplot(3, 2, 1);
plot(time, outputs(:, 1:2));
title('控制输出响应');
xlabel('时间 (s)');
ylabel('幅值');
legend('输出 1', '输出 2');
grid on;

% 子图 2: 状态变量
subplot(3, 2, 2);
plot(time, states(:, 1:3));
title('系统状态');
xlabel('时间 (s)');
ylabel('状态值');
legend('状态 1', '状态 2', '状态 3');
grid on;

% 子图 3: 控制误差
subplot(3, 2, 3);
error = calculateError(outputs);
plot(time, error);
title('控制误差');
xlabel('时间 (s)');
ylabel('误差');
grid on;

% 子图 4: 参数自适应
subplot(3, 2, 4);
if isfield(simOut.logsout, 'parameters')
    params = simOut.logsout.parameters.Values.Data;
    plot(time, squeeze(params(:,1,:)));
    title('自适应参数变化');
    xlabel('时间 (s)');
    ylabel('参数值');
    grid on;
end

% 子图 5: 性能指标
subplot(3, 2, 5);
metrics = calculatePerformanceMetrics(simOut);
bar(metrics.values);
title('性能指标');
xlabel('指标');
ylabel('值');
set(gca, 'XTickLabel', metrics.names);
grid on;

% 子图 6: 频率响应
subplot(3, 2, 6);
[mag, phase, freq] = calculateFrequencyResponse(simOut);
semilogx(freq, 20*log10(mag));
title('频率响应');
xlabel('频率 (Hz)');
ylabel('幅值 (dB)');
grid on;

% 保存图形
saveas(gcf, 'MODEL_B_analysis.png');
```

## 模型验证

### 验证测试套件

#### 测试 1: 稳定性测试
**目的**: 验证闭环系统稳定性

**测试方法**:
1. 在不同工作点初始化系统
2. 施加阶跃和脉冲扰动
3. 分析系统响应

**验收标准**:
- 所有工作点均稳定
- 超调量 < 10%
- 调节时间符合规格要求

#### 测试 2: 鲁棒性测试
**目的**: 验证系统对参数不确定性的鲁棒性

**测试方法**:
1. 在参数空间随机采样
2. 运行蒙特卡洛仿真
3. 统计性能指标

**验收标准**:
- 90% 的仿真满足性能要求
- 最坏情况性能下降不超过 20%

#### 测试 3: 故障容错测试
**目的**: 验证故障检测和恢复能力

**测试方法**:
1. 注入不同类型的故障
2. 观察故障检测时间
3. 评估系统恢复性能

**验收标准**:
- 故障检测时间 < 0.5 秒
- 性能下降在可接受范围内
- 系统不进入不稳定状态

#### 测试 4: 实时性测试
**目的**: 验证模型实时执行能力

**测试方法**:
1. 测量单步计算时间
2. 验证满足实时约束
3. 测试代码生成性能

**验收标准**:
- 单步计算时间 < 采样时间
- 代码生成满足目标平台要求

## 性能指标

### 控制性能
- **稳态误差**: < 0.5% (各通道)
- **跟踪误差**: < 1% (正弦跟踪)
- **干扰抑制**: > 40 dB (在 1Hz)
- **命令响应**: 上升时间 < 0.1 秒

### 计算性能
- **最大采样频率**: 200 Hz (基于标准硬件)
- **内存使用**: < 200 MB
- **代码效率**: 生成代码 < 50 KB (ROM)
- **实时性能**: 单步计算 < 2 ms (在 500 MHz 处理器)

### 鲁棒性指标
- **增益裕度**: > 6 dB
- **相位裕度**: > 45°
- **稳定时间**: < 2 秒 (对于 10% 阶跃)
- **参数变化容忍度**: ±30%

## 故障排除

### 常见问题

#### 问题 1: 优化求解失败
**症状**: 优化求解器返回错误或无法找到可行解

**可能原因**:
1. 约束条件冲突
2. 优化问题非凸
3. 数值问题（条件数过大）

**解决方案**:
1. 检查并调整约束条件
2. 使用不同的优化算法
3. 添加正则化项改善数值条件

#### 问题 2: 自适应参数发散
**症状**: 自适应参数无限增大或振荡

**可能原因**:
1. 自适应增益过大
2. 持续激励不足
3. 数值积分问题

**解决方案**:
1. 减小自适应增益
2. 添加持续激励信号
3. 使用泄漏积分或参数投影

#### 问题 3: 状态估计偏差
**症状**: 估计状态与实际状态存在较大偏差

**可能原因**:
1. 过程或测量噪声协方差设置不当
2. 模型失配
3. 滤波器初始化错误

**解决方案**:
1. 调整噪声协方差矩阵
2. 改进系统模型
3. 改进滤波器初始化

#### 问题 4: 实时性能不足
**症状**: 单步计算时间超过采样时间

**可能原因**:
1. 模型过于复杂
2. 优化问题规模过大
3. 算法实现效率低

**解决方案**:
1. 简化模型或降低模型阶次
2. 减小预测时域或控制时域
3. 使用更高效的算法实现

### 调试工具

#### 内置调试功能
```matlab
% 启用详细日志
set_param('MODEL_B', 'Debug', 'on');

% 设置调试断点
set_param('MODEL_B/AdaptiveController', 'Breakpoints', 'on');

% 查看内部信号
addSignalLogging('MODEL_B', {'AdaptiveController/parameters', ...
                              'OptimizationEngine/cost'});

% 性能分析
Simulink.profiler.start;
sim('MODEL_B.slx');
Simulink.profiler.stop;
Simulink.profiler.report;
```

#### 自定义监控
```matlab
% 创建自定义监控脚本
function monitorModelB(simOut)
    % 提取关键数据
    time = simOut.tout;
    outputs = simOut.yout;
    
    % 实时监控指标
    metrics = struct();
    metrics.rise_time = calculateRiseTime(outputs);
    metrics.settling_time = calculateSettlingTime(outputs);
    metrics.overshoot = calculateOvershoot(outputs);
    metrics.rmse = calculateRMSE(outputs);
    
    % 显示监控结果
    fprintf('性能指标:\n');
    fprintf('  上升时间: %.3f s\n', metrics.rise_time);
    fprintf('  调节时间: %.3f s\n', metrics.settling_time);
    fprintf('  超调量: %.2f%%\n', metrics.overshoot*100);
    fprintf('  RMSE: %.4f\n', metrics.rmse);
    
    % 检查性能阈值
    checkPerformanceThresholds(metrics);
end
```

## 代码生成

### 生成配置
```matlab
% 配置代码生成选项
cfg = coder.config('lib');
cfg.TargetLang = 'C';
cfg.GenerateReport = true;
cfg.ReportPotentialDifferences = false;
cfg.HardwareImplementation.ProdHWDeviceType = 'Intel->x86-64 (Linux 64)';

% 配置模型参数
set_param('MODEL_B', 'SystemTargetFile', 'ert.tlc');
set_param('MODEL_B', 'TargetLang', 'C');
set_param('MODEL_B', 'GenerateSampleERTMain', 'on');
set_param('MODEL_B', 'ERTCustomFileTemplate', 'ert_code_template.cgt');

% 生成代码
rtwbuild('MODEL_B');
```

### 生成代码结构
```
MODEL_B_ert_rtw/
├── MODEL_B.c          # 主模型代码
├── MODEL_B.h          # 模型头文件
├── MODEL_B_private.h  # 私有头文件
├── MODEL_B_types.h    # 类型定义
├── rtmodel.h          # 实时模型头文件
├── MODEL_B_data.c     # 模型数据
├── MODEL_B_utils.c    # 工具函数
├── ert_main.c         # 示例主程序
└── html/              # 详细生成报告
    ├── index.html
    ├── files.html
    └── ...
```

### 目标集成

#### 集成步骤
1. **文件复制**: 将生成的代码文件复制到目标项目
2. **接口适配**: 实现目标特定的 I/O 接口
3. **调度集成**: 集成到目标系统的任务调度器
4. **测试验证**: 使用 PIL 或 HIL 测试

#### 接口函数
```c
/* 模型接口函数 */
void MODEL_B_initialize(void);     // 初始化模型
void MODEL_B_step(void);           // 执行一步计算
void MODEL_B_terminate(void);      // 终止模型

/* 数据接口函数 */
void MODEL_B_set_input(real_T *inputs);     // 设置输入
void MODEL_B_get_output(real_T *outputs);   // 获取输出
void MODEL_B_get_state(real_T *states);     // 获取状态
```

## 版本历史

### v1.0.0 (2024-06-15)
- 正式发布版本
- 包含所有高级控制功能
- 通过完整验证测试套件
- 支持代码生成和实时部署

### v0.9.5 (2024-05-30)
- 改进优化算法效率
- 增强故障诊断能力
- 添加更多验证测试
- 完善文档和示例

### v0.9.0 (2024-05-15)
- Beta 测试版本
- 实现主要控制算法
- 基本验证通过
- 初步代码生成支持

### v0.8.0 (2024-04-30)
- Alpha 测试版本
- 完成模型框架
- 实现核心算法
- 初步功能测试

## 相关文档

- [MODEL_A 文档](./MODEL_A.md) - 基础模型文档
- [系统架构](../architecture.md) - 整体系统设计
- [高级教程](../tutorials/tutorial-advanced.md) - 高级使用指南
- [API 文档](../api/index.md) - 项目 API 参考
- [示例数据](../examples/example-data/) - 示例模型和脚本
