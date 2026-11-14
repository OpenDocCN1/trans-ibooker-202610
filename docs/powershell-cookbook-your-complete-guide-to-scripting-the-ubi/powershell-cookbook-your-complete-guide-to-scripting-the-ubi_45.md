## 目录

1.  前言
2.  序言
    1.  适读人群
    2.  本书结构
    3.  使用本书所需条件
    4.  本书约定
    5.  获取本书数字版
    6.  O'Reilly 在线学习
    7.  联系我们
    8.  致谢
3.  导览
4.  PowerShell 指引导览
    1.  简介
    2.  交互式 Shell
    3.  结构化命令
    4.  对象的深度集成
    5.  管理员作为一等用户
    6.  可组合命令
    7.  保护您免受自身伤害的技术
    8.  常见发现命令
    9.  无处不在的脚本编写
    10.  临时开发
    11.  桥接技术
    12.  通过提供程序进行命名空间导航
    13.  更多内容
5.  基础
6.  1. PowerShell 交互式 Shell
    1.  1.0. 简介
    2.  1.1. 安装 PowerShell Core
    3.  1.2. 运行程序、脚本和现有工具
    4.  1.3. 运行 PowerShell 命令
    5.  1.4. 解决调用原生可执行文件的错误
    6.  1.5. 为参数提供默认值
    7.  1.6. 调用长时间运行或后台命令
    8.  1.7. 程序：监视命令的变化
    9.  1.8. 作业完成时通知自己
    10.  1.9. 自定义 Shell、配置文件和提示符
    11.  1.10. 自定义 PowerShell 的用户输入行为
    12.  1.11. 自定义 PowerShell 的命令解析行为
    13.  1.12. 查找完成任务的命令
    14.  1.13. 获取命令帮助
    15.  1.14. 更新系统帮助内容
    16.  1.15. 程序：搜索帮助文本
    17.  1.16. 在特定位置启动 PowerShell
    18.  1.17. 从 PowerShell 外部调用 PowerShell 命令或脚本
    19.  1.18. 理解并自定义 PowerShell 的 Tab 补全





## 第 1 章

### 1.19 程序：学习常用命令的别名 `#ch01.xhtml#program_learn_aliases_for_common_command`

### 1.20 程序：学习常用参数的别名 `#ch01.xhtml#get-parameter-aliases`

### 1.21 访问和管理控制台历史记录 `#ch01.xhtml#access_and_manage_your_console_history`

### 1.22 程序：从会话历史记录创建脚本 `#ch01.xhtml#copy-history`

### 1.23 从会话历史记录调用命令 `#ch01.xhtml#invoke-command-history`

### 1.24 程序：在格式化输出中搜索模式 `#ch01.xhtml#select-textoutput`

### 1.25 以交互方式查看和处理命令输出 `#ch01.xhtml#view-command-output`

### 1.26 程序：以交互方式查看和探索对象 `#ch01.xhtml#view_explore_objects`

### 1.27 记录 Shell 会话的逐字记录 `#ch01.xhtml#record_a_transcript_of_your_shell_sessio`

### 1.28 使用其他命令扩展 Shell `#ch01.xhtml#extend_your_shell_with_additional_commands`

### 1.29 查找并安装其他 PowerShell 脚本和模块 `#ch01.xhtml#install_module_gallery`

### 1.30 使用来自自定义 Shell 的命令 `#ch01.xhtml#use_commands_customized_shells`

### 1.31 在会话间保存状态 `#ch01.xhtml#save-state-exiting`

## 2. 管道 `#ch02.xhtml#pipelines`

### 2.0 简介 `#ch02.xhtml#introduction-id002`

### 2.1 根据成功或错误链接命令 `#ch02.xhtml#pipeline_commands_based_on_success`

### 2.2 筛选列表或命令输出中的项目 `#ch02.xhtml#filter_items_in_a_list_or_command_output`

### 2.3 按名称对数据进行分组和透视 `#ch02.xhtml#group_and_pivot_data`

### 2.4 以交互方式筛选对象列表 `#ch02.xhtml#program_interactively_filter_lists_of_ob`

### 2.5 处理列表或命令输出中的每个项目 `#ch02.xhtml#work_with_each_item_in_a_list_or_command`

### 2.6 自动化数据密集型任务 `#ch02.xhtml#automate_data-intensive_tasks`

### 2.7 拦截管道的各个阶段 `#ch02.xhtml#intercept-powershell-pipeline`

### 2.8 自动捕获管道输出 `#ch02.xhtml#capture-pipeline-objects`

### 2.9 捕获并重定向二进制进程输出 `#ch02.xhtml#pipe-binary-data`

## 3. 变量和对象 `#ch03.xhtml#variables_and_objects`

### 3.0 简介 `#ch03.xhtml#introduction-id003`

### 3.1 以列表形式显示项目的属性 `#ch03.xhtml#display_the_properties_of_an_item_as_a_l`

### 3.2 以表格形式显示项目的属性 `#ch03.xhtml#display_the_properties_of_an_item_as_a_t`

### 3.3 在变量中存储信息 `#ch03.xhtml#store_information_in_variables`

### 3.4 访问环境变量 `#ch03.xhtml#access_environment_variables`

### 3.5 程序：保留批处理文件设置的环境变量更改 `#ch03.xhtml#program_retain_changes_to_environment_va`

### 3.6 控制变量和其他项目的访问权限和作用域 `#ch03.xhtml#control_access_and_scope_of_variables_an`

### 3.7 程序：创建动态变量 `#ch03.xhtml#program_create_dynamic_variables`

### 3.8 使用 .NET 对象 `#ch03.xhtml#work_with_net_objects`

### 3.9 创建 .NET 对象的实例 `#ch03.xhtml#create_an_instance_of_a_net_object`

### 3.10 创建泛型对象的实例 `#ch03.xhtml#program_create_instances_of_generic_obje`

### 3.11 使用 COM 对象 `#ch03.xhtml#use_a_com_object`

### 3.12 了解类型和对象 `#ch03.xhtml#learn_about_types_and_objects`

### 3.13 获取关于类型和对象的详细文档 `#ch03.xhtml#get_detailed_documentation_about_types_a`

### 3.14 向对象添加自定义方法和属性 `#ch03.xhtml#add_custom_methods_and_properties_to_obj`

### 3.15 创建并初始化自定义对象 `#ch03.xhtml#create_and_initialize_custom_objects`

### 3.16 向类型添加自定义方法和属性 `#ch03.xhtml#add_custom_methods_and_properties_to_typ`

### 3.17 为类型定义自定义格式 `#ch03.xhtml#add_formatting_to_type`

## 4. 循环和流程控制 `#ch04.xhtml#looping_and_flow_control`

### 4.0 简介 `#ch04.xhtml#introduction-id004`

### 4.1 使用比较和逻辑运算符进行决策 `#ch04.xhtml#make_decisions_with_comparison_and_logic`

### 4.2 使用条件语句调整脚本流程 `#ch04.xhtml#adjust_script_flow_using_conditional_sta`

### 4.3 使用 Switch 语句管理大型条件语句 `#ch04.xhtml#manage_large_conditional_statements_with`

### 4.4 使用循环重复操作 `#ch04.xhtml#repeat_operations_with_loops`

### 4.5 并行处理耗时操作 `#ch04.xhtml#run_looping_statement_parallel`

### 4.6 添加暂停或延迟 `#ch04.xhtml#add_a_pause_or_delay`

## 5. 字符串和非结构化文本 `#ch05.xhtml#strings_and_unstructured_text`

### 5.0 简介 `#ch05.xhtml#introduction-id005`

### 5.1 创建字符串 `#ch05.xhtml#create_a_string`

### 5.2 创建多行或格式化字符串 `#ch05.xhtml#create_a_multiline_or_formatted_string`

### 5.3 在字符串中放置特殊字符 `#ch05.xhtml#place_special_characters_in_a_string`

### 5.4 在字符串中插入动态信息 `#ch05.xhtml#insert_dynamic_information_in_a_string`

### 5.5 防止字符串包含动态信息 `#ch05.xhtml#prevent_a_string_from_including_dynamic_`

### 5.6 在字符串中放置格式化信息 `#ch05.xhtml#place_formatted_information_in_a_string`

### 5.7 在字符串中搜索文本或模式 `#ch05.xhtml#search_a_string_for_text_or_a_pattern`

### 5.8 替换字符串中的文本 `#ch05.xhtml#replace_text_in_a_string`

### 5.9 按文本或模式拆分字符串 `#ch05.xhtml#split_string_on_text`

### 5.10 将字符串合并为更大的字符串 `#ch05.xhtml#combine_string`

### 5.11 将字符串转换为大写或小写 `#ch05.xhtml#convert_a_string_to_upperlowercase`

### 5.12 裁剪字符串 `#ch05.xhtml#trim_a_string`

### 5.13 为输出格式化日期 `#ch05.xhtml#format_a_date_for_output`

### 5.14 在不同格式间转换字符串 `#ch05.xhtml#convert_string_between_formats`

### 5.15 将文本流转换为对象 `#ch05.xhtml#convert_text_streams_to_objects`

### 5.16 生成大型报告和文本流 `#ch05.xhtml#generate_large_reports_and_text_streams`

### 5.17 生成源代码和其他重复性文本 `#ch05.xhtml#generate_source_code`

## 6. 计算和数学 `#ch06.xhtml#calculations_and_math`

### 6.0 简介 `#ch06.xhtml#introduction-id006`

### 6.1 执行简单算术运算 `#ch06.xhtml#perform_simple_arithmetic`

### 6.2 执行复杂算术运算 `#ch06.xhtml#perform_complex_arithmetic`

### 6.3 测量列表的统计属性 `#ch06.xhtml#measure_statistical_properties_of_a_list`

### 6.4 将数字作为二进制处理 `#ch06.xhtml#work_with_numbers_as_binary`

### 6.5 使用管理常量简化数学运算 `#ch06.xhtml#simplify_math_with_administrative_consta`

### 6.6 在不同进制间转换数字 `#ch06.xhtml#convert_numbers_between_bases`

## 7. 列表、数组和哈希表 `#ch07.xhtml#lists_arrays_and_hashtables`

### 7.0 简介 `#ch07.xhtml#introduction-id011`

### 7.1 创建项目数组或列表 `#ch07.xhtml#create_an_array_or_list_of_items`

### 7.2 创建不规则或多维数组 `#ch07.xhtml#create_a_jagged_or_multidimensional_arra`

### 7.3 访问数组元素 `#ch07.xhtml#access_elements_of_an_array`

### 7.4 遍历数组的每个元素 `#ch07.xhtml#visit_each_element_of_an_array`




## 第 7 章：数组和列表（续）

### 7.5 对数组或列表中的项进行排序 #ch07.xhtml#sort_an_array_or_list_of_items

### 7.6 确定数组是否包含某项 #ch07.xhtml#determine_whether_an_array_contains_an_i

### 7.7 合并两个数组 #ch07.xhtml#combine_two_arrays

### 7.8 查找数组中与值匹配的项 #ch07.xhtml#find_items_in_an_array_that_match_a_valu

### 7.9 比较两个列表 #ch07.xhtml#compare_two_lists

### 7.10 从数组中移除元素 #ch07.xhtml#remove_elements_from_an_array

### 7.11 查找数组中大于或小于某值的项 #ch07.xhtml#find_items_in_an_array_greater_or_less_t

### 7.12 使用 `ArrayList` 类执行高级数组任务 #ch07.xhtml#use_the_arraylist_class_for_advanced_arr

### 7.13 创建 `Hashtable` 或关联数组 #ch07.xhtml#create_a_hashtable_or_associative_array

### 7.14 按键或值对 `Hashtable` 进行排序 #ch07.xhtml#sort_a_hashtable_by_key_or_value

## 第 8 章：实用任务 #ch08.xhtml#utility_scripting

### 8.0 简介 #ch08.xhtml#introduction-id007

### 8.1 获取系统日期和时间 #ch08.xhtml#get_the_system_date_and_time

### 8.2 测量命令的持续时间 #ch08.xhtml#measure_the_duration_of_a_command

### 8.3 从剪贴板读取和写入 #ch08.xhtml#read_write_windows_clipboard

### 8.4 生成随机数或对象 #ch08.xhtml#generate_random_number_object

### 8.5 在时区之间转换时间 #ch08.xhtml#convert_between_time_zones

### 8.6 程序：搜索 Windows 开始菜单 #ch08.xhtml#program_search_the_windows_start_menu

### 8.7 程序：显示着色的脚本内容 #ch08.xhtml#program_show_colorized_content

# 第 3 部分：常见任务 #part03.xhtml#part3

## 第 9 章：简单文件 #ch09.xhtml#simple_files

### 9.0 简介 #ch09.xhtml#I_sect11_id528085

### 9.1 获取文件内容 #ch09.xhtml#get_the_content_of_a_file

### 9.2 将命令输出存储到文件中 #ch09.xhtml#store_the_output_of_a_command_into_a_fil

### 9.3 向文件末尾添加信息 #ch09.xhtml#add_information_to_the_end_of_a_file

### 9.4 在文件中搜索文本或模式 #ch09.xhtml#search_a_file_for_text_or_a_pattern

### 9.5 解析和管理基于文本的日志文件 #ch09.xhtml#parse_and_manage_text-based_logfiles

### 9.6 解析和管理二进制文件 #ch09.xhtml#parse_and_manage_binary_files

### 9.7 创建和管理临时文件 #ch09.xhtml#create_manage_temporary_file

### 9.8 在文件中搜索和替换文本 #ch09.xhtml#search_and_replace_text_in_a_file

### 9.9 程序：获取文件的编码 #ch09.xhtml#program_get_encoding_file

### 9.10 查看内容的十六进制表示 #ch09.xhtml#format_hex

## 第 10 章：结构化文件 #ch10.xhtml#structured_files

### 10.0 简介 #ch10.xhtml#introduction-id008

### 10.1 访问 `XML` 文件中的信息 #ch10.xhtml#access_information_in_an_xml_file

### 10.2 对 `XML` 执行 `XPath` 查询 #ch10.xhtml#perform_an_xpath_query_against_an_xml_fi

### 10.3 将对象转换为 `XML` #ch10.xhtml#convert_command_output_to_xml

### 10.4 修改 `XML` 文件中的数据 #ch10.xhtml#modify_data_in_an_xml_file

### 10.5 轻松导入和导出结构化数据 #ch10.xhtml#easily_import_and_export_your_structured

### 10.6 将命令输出存储到 `CSV` 或分隔文件中 #ch10.xhtml#store_the_output_of_a_command_in_a_csv_f

### 10.7 从文件导入 `CSV` 和分隔数据 #ch10.xhtml#import_structured_data_from_a_csv_file

### 10.8 管理 `JSON` 数据流 #ch10.xhtml#import_export_json

### 10.9 使用 `Excel` 管理命令输出 #ch10.xhtml#use_excel_to_manage_command_output

### 10.10 解析和解释 `PowerShell` 脚本 #ch10.xhtml#parse_and_interpret_powershell_scripts

## 第 11 章：代码重用 #ch11.xhtml#code_reuse

### 11.0 简介 #ch11.xhtml#introduction-id010

### 11.1 编写 `Script` #ch11.xhtml#write_a_script

### 11.2 编写 `Function` #ch11.xhtml#write_a_function

### 11.3 为命令名称查找合适的 `Verb` #ch11.xhtml#find_a_verb

### 11.4 编写 `ScriptBlock` #ch11.xhtml#write_a_script_block

### 11.5 从 `Script`、`Function` 或 `ScriptBlock` 返回数据 #ch11.xhtml#return_data_from_a_script_function_or_sc

### 11.6 在 `Module` 中打包常用命令 #ch11.xhtml#place_common_functions_in_a_library

### 11.7 编写维护状态的命令 #ch11.xhtml#write_commands_maintain_global_state

### 11.8 从 `Module` 中选择性导出命令 #ch11.xhtml#selectively_export_commands_from_module

### 11.9 诊断和交互内部 `Module` 状态 #ch11.xhtml#diagnose_interact_module_state

### 11.10 处理移除 `Module` 时的清理任务 #ch11.xhtml#clean_module_state

### 11.11 访问 `Script`、`Function` 或 `ScriptBlock` 的参数 #ch11.xhtml#access_arguments_of_a_script_function_or

### 11.12 向 `Parameters` 添加验证 #ch11.xhtml#add_validation_to_parameters

### 11.13 使用局部变量接受 `ScriptBlock` 参数 #ch11.xhtml#accept_script_block_parameters_with_local

### 11.14 动态组合命令 `Parameters` #ch11.xhtml#dynamically_compose_command_parameters

### 11.15 提供 `-WhatIf`、`-Confirm` 和其他 `Cmdlet` 功能 #ch11.xhtml#support_whatif_confirm

### 11.16 为 `Scripts` 或 `Functions` 添加帮助 #ch11.xhtml#add_help_scripts_functions

### 11.17 向 `Function` 或 `ScriptBlock` 添加自定义标记 #ch11.xhtml#add_custom_tags_to_a_function

### 11.18 访问 `Script` 的管道输入 #ch11.xhtml#access_pipeline_input

### 11.19 使用 `Cmdlet` 关键字编写面向管道的 `Scripts` #ch11.xhtml#write_pipeline-oriented_scripts_with_cmd

### 11.20 编写面向管道的 `Function` #ch11.xhtml#write_a_pipeline-oriented_function

### 11.21 组织 `Scripts` 以提高可读性 #ch11.xhtml#organize_scripts_improved_readability

### 11.22 调用动态命名的命令 #ch11.xhtml#invoke-dynamically-named-commands

### 11.23 程序：增强或扩展现有 `Cmdlet` #ch11.xhtml#enhance_extend_existing_cmdlet

## 第 12 章：支持 Internet 的脚本 #ch12.xhtml#internet_enabled_scripts

### 12.0 简介 #ch12.xhtml#introduction-id009

### 12.1 从 `FTP` 或 Internet 站点下载文件 #ch12.xhtml#download_a_file_from_the_internet

### 12.2 将文件上传到 `FTP` 站点 #ch12.xhtml#upload_a_file_from_the_internet

### 12.3 程序：解析 Internet 重定向的目标 #ch12.xhtml#resolve_uri_target

### 12.4 从 Internet 下载网页 #ch12.xhtml#download_a_web_page_from_the_internet

### 12.5 解析和分析来自 Internet 的网页 #ch12.xhtml#parse_analyze_web_page

### 12.6 为 Web 应用程序 `Session` 编写脚本 #ch12.xhtml#script_web_session

### 12.7 与基于 `REST` 的 Web API 交互 #ch12.xhtml#interact_rest_api

### 12.8 连接到 Web 服务 #ch12.xhtml#connect_to_a_webservice

### 12.9 交互和管理远程 `SSL` 证书 #ch12.xhtml#test_retrieve_ssl_certificate

### 12.10 将命令输出导出为网页 #ch12.xhtml#export_command_output_as_a_web_page

### 12.11 发送电子邮件 #ch12.xhtml#send_an_email

### 12.12 程序：监控网站运行时间 #ch12.xhtml#program_monitor_website

### 12.13 程序：与 Internet 协议交互 #ch12.xhtml#program_interact_with_internet_protocols

## 第 13 章：用户交互 #ch13.xhtml#user_interaction




## 13. 用户交互

### 13.0. 简介
### 13.1. 读取用户输入的一行
### 13.2. 读取用户输入的一个按键
### 13.3. 程序：向用户显示菜单
### 13.4. 向用户显示消息和输出
### 13.5. 为长时间运行的任务提供进度更新
### 13.6. 编写区域性感知脚本
### 13.7. 在脚本输出中支持其他语言
### 13.8. 程序：使用备用区域性设置调用脚本块
### 13.9. 访问宿主 UI 的功能
### 13.10. 为脚本添加图形用户界面
### 13.11. 程序：为脚本添加控制台 UI
### 13.12. 与 `MTA` 对象交互

## 14. 调试

### 14.0. 简介
### 14.1. 预防常见脚本错误
### 14.2. 为脚本编写单元测试
### 14.3. 跟踪脚本执行
### 14.4. 设置脚本断点
### 14.5. 脚本遇到错误时进行调试
### 14.6. 创建条件断点
### 14.7. 调试时检查系统状态
### 14.8. 在远程计算机上调试脚本
### 14.9. 程序：监视表达式的变化
### 14.10. 在另一个进程中调试脚本
### 14.11. 程序：获取脚本代码覆盖率

## 15. 跟踪与错误管理

### 15.0. 简介
### 15.1. 确定上一条命令的状态
### 15.2. 查看命令生成的错误
### 15.3. 管理命令的错误输出
### 15.4. 程序：解析错误
### 15.5. 配置调试、详细和进度输出
### 15.6. 处理警告、错误和终止错误
### 15.7. 输出警告、错误和终止错误
### 15.8. 分析脚本的性能配置文件

## 16. 环境感知

### 16.0. 简介
### 16.1. 查看和修改环境变量
### 16.2. 修改用户或系统路径
### 16.3. 访问有关命令调用的信息
### 16.4. 程序：检查 `InvocationInfo` 变量
### 16.5. 查找脚本名称
### 16.6. 查找脚本位置
### 16.7. 查找常见系统路径的位置
### 16.8. 获取当前位置
### 16.9. 安全地从组件构建文件路径
### 16.10. 与 PowerShell 的全局环境交互
### 16.11. 确定 PowerShell 版本信息
### 16.12. 测试管理权限

## 17. 扩展 PowerShell 的能力

### 17.0. 简介
### 17.1. 使用 `COM` 脚本接口自动化程序
### 17.2. 程序：查询 `SQL` 数据源
### 17.3. 访问 Windows 性能计数器
### 17.4. 访问 Windows `API` 函数
### 17.5. 程序：调用简单的 Windows `API`
### 17.6. 定义或扩展 `.NET` 类
### 17.7. 在 PowerShell 脚本中添加内联 `C#`
### 17.8. 访问 `.NET SDK` 库
### 17.9. 创建你自己的 PowerShell `Cmdlet`
### 17.10. 向你自己的程序添加 PowerShell 脚本功能

## 18. 安全与脚本签名

### 18.0. 简介
### 18.1. 通过执行策略启用脚本
### 18.2. 启用 PowerShell 安全日志记录
### 18.3. 禁用 `UNC` 路径的警告
### 18.4. 对 PowerShell 脚本、模块或格式化文件进行签名
### 18.5. 创建自签名证书
### 18.6. 在企业中管理 PowerShell 安全性
### 18.7. 按发布者、路径或哈希阻止脚本
### 18.8. 验证 PowerShell 脚本的数字签名
### 18.9. 安全地处理敏感信息
### 18.10. 安全地请求用户名和密码
### 18.11. 以另一用户身份启动进程
### 18.12. 程序：运行临时提权的命令
### 18.13. 安全地在磁盘上存储凭据
### 18.14. 访问用户和计算机证书
### 18.15. 程序：搜索证书存储
### 18.16. 添加和删除证书
### 18.17. 以 `SDDL` 形式管理安全描述符




## 第 18 章：高级远程处理和安全

### 18.18 创建特定任务的远程处理端点
[`#ch18.xhtml#create_task_specific_endpoint`]

### 18.19 限制 PowerShell 的交互式使用
[`#ch18.xhtml#limit_interactive_use_powershell`]

### 18.20 检测和防止代码注入漏洞
[`#ch18.xhtml#detect_prevent_code_injection`]

### 18.21 获取文件的加密哈希值
[`#ch18.xhtml#get_the_hash_of_a_file`]

### 18.22 捕获并验证文件集的完整性
[`#ch18.xhtml#capture_verify_file_integrity`]

## 第 19 章：Visual Studio Code

### 19.0 简介
[`#ch19.xhtml#ch19_id2311048`]

### 19.1 调试脚本
[`#ch19.xhtml#debug_script_ise`]

### 19.2 连接到远程计算机
[`#ch19.xhtml#connect_remote_computer_ise`]

### 19.3 通过其对象模型与 Visual Studio Code 交互
[`#ch19.xhtml#interact_vscode_object_model`]

### 19.4 快速插入脚本片段
[`#ch19.xhtml#insert_script_snippet`]

## 部分：管理员任务
[`#part04.xhtml#part4`]

## 第 20 章：文件和目录

### 20.0 简介
[`#ch20.xhtml#introduction-id017`]

### 20.1 确定并更改当前位置
[`#ch20.xhtml#determine_the_current_location`]

### 20.2 获取目录中的文件
[`#ch20.xhtml#get_the_files_in_a_directory`]

### 20.3 查找在特定日期之前修改的所有文件
[`#ch20.xhtml#find_all_files_modified_before_a_certain`]

### 20.4 清除文件内容
[`#ch20.xhtml#clear_or_remove_a_file`]

### 20.5 管理并更改文件属性
[`#ch20.xhtml#manage_and_change_the_attributes_of_a_fi`]

### 20.6 查找匹配模式的文件
[`#ch20.xhtml#find_files_that_match_a_pattern`]

### 20.7 管理包含特殊字符的文件
[`#ch20.xhtml#manage_files_that_include_special_charac`]

### 20.8 程序：获取磁盘使用信息
[`#ch20.xhtml#program_get_disk_usage_information`]

### 20.9 监视文件更改
[`#ch20.xhtml#monitor_a_file_for_changes`]

### 20.10 获取 DLL 或可执行文件的版本
[`#ch20.xhtml#get_version_dll_executable`]

### 20.11 创建目录
[`#ch20.xhtml#create_a_directory`]

### 20.12 删除文件或目录
[`#ch20.xhtml#remove_a_file_or_directory`]

### 20.13 重命名文件或目录
[`#ch20.xhtml#rename_a_file_or_directory`]

### 20.14 移动文件或目录
[`#ch20.xhtml#move_a_file_or_directory`]

### 20.15 创建和映射 PowerShell 驱动器
[`#ch20.xhtml#create_map_powershell_drive`]

### 20.16 访问长文件名和目录名
[`#ch20.xhtml#access_long_file_name`]

### 20.17 解除文件阻止
[`#ch20.xhtml#unblock_file`]

### 20.18 与备用数据流交互
[`#ch20.xhtml#alternate_data_streams`]

### 20.19 程序：移动或删除锁定的文件
[`#ch20.xhtml#program_move_remove_locked_file`]

### 20.20 获取文件或目录的 ACL
[`#ch20.xhtml#get_the_acl_of_a_file_or_directory`]

### 20.21 设置文件或目录的 ACL
[`#ch20.xhtml#set_the_acl_of_a_file_or_directory`]

### 20.22 程序：向文件添加扩展文件属性
[`#ch20.xhtml#program_add_extended_file_properties_to_`]

### 20.23 管理 ZIP 存档
[`#ch20.xhtml#manage_zip_archive`]

## 第 21 章：Windows 注册表

### 21.0 简介
[`#ch21.xhtml#introduction-id018`]

### 21.1 导航注册表
[`#ch21.xhtml#navigate_the_registry`]

### 21.2 查看注册表项
[`#ch21.xhtml#view_a_registry_key`]

### 21.3 修改或删除注册表键值
[`#ch21.xhtml#modify_or_remove_a_registry_key_value`]

### 21.4 创建注册表键值
[`#ch21.xhtml#create_a_registry_key_value`]

### 21.5 删除注册表项
[`#ch21.xhtml#remove_a_registry_key`]

### 21.6 安全地合并相关的注册表修改
[`#ch21.xhtml#safely_combine_related_registry_modifications`]

### 21.7 将站点添加到 Internet Explorer 安全区域
[`#ch21.xhtml#add_a_site_to_an_internet_explorer_secur`]

### 21.8 修改 Internet Explorer 设置
[`#ch21.xhtml#modify_internet_explorer_settings`]

### 21.9 程序：搜索 Windows 注册表
[`#ch21.xhtml#program_search_the_windows_registry`]

### 21.10 获取注册表项的 ACL
[`#ch21.xhtml#get_the_acl_of_a_registry_key`]

### 21.11 设置注册表项的 ACL
[`#ch21.xhtml#set_the_acl_of_a_registry_key`]

### 21.12 处理远程计算机的注册表
[`#ch21.xhtml#work_with_the_registry_of_a_remote_compu`]

### 21.13 程序：从远程计算机获取注册表项
[`#ch21.xhtml#program_get_registry_items_from_remote_m`]

### 21.14 程序：获取远程注册表项的属性
[`#ch21.xhtml#program_get_properties_of_remote_registr`]

### 21.15 程序：设置远程注册表项的属性
[`#ch21.xhtml#program_set_properties_of_remote_registr`]

### 21.16 发现程序的注册表设置
[`Images/ch21.html#discover_registry_settings_for_programs`]

## 第 22 章：比较数据

### 22.0 简介
[`#ch22.xhtml#introduction-id019`]

### 22.1 比较两个命令的输出
[`#ch22.xhtml#compare_the_output_of_two_commands`]

### 22.2 确定两个文件之间的差异
[`#ch22.xhtml#determine_the_differences_between_two_fi`]

## 第 23 章：事件日志

### 23.0 简介
[`#ch23.xhtml#introduction-id020`]

### 23.1 列出所有事件日志
[`#ch23.xhtml#list_all_event_logs`]

### 23.2 从事件日志获取最早的条目
[`#ch23.xhtml#get_the_newest_entries_from_an_event_log`]

### 23.3 查找包含特定文本的事件日志条目
[`#ch23.xhtml#find_event_log_entries_with_specific_tex`]

### 23.4 检索并筛选事件日志条目
[`#ch23.xhtml#retrieve_a_specific_event_log_entry`]

### 23.5 按频率查找事件日志条目
[`#ch23.xhtml#find_event_log_entries_by_their_frequenc`]

### 23.6 备份事件日志
[`#ch23.xhtml#back_up_an_event_log`]

### 23.7 创建或删除事件日志
[`#ch23.xhtml#create_or_remove_an_event_log`]

### 23.8 写入事件日志
[`#ch23.xhtml#write_to_an_event_log`]

### 23.9 为 Windows 事件日志条目运行 PowerShell 脚本
[`#ch23.xhtml#respond_eventlog_events`]

### 23.10 清除或维护事件日志
[`#ch23.xhtml#clear_maintain_event_log`]

### 23.11 访问远程计算机的事件日志
[`#ch23.xhtml#access_event_logs_of_a_remote_machine`]

## 第 24 章：进程

### 24.0 简介
[`#ch24.xhtml#introduction-id021`]

### 24.1 列出当前正在运行的进程
[`#ch24.xhtml#list_currently_running_processes`]

### 24.2 启动与文档关联的应用程序
[`#ch24.xhtml#launch_application_associated`]

### 24.3 启动进程
[`#ch24.xhtml#launch_a_process`]

### 24.4 停止进程
[`#ch24.xhtml#stop_a_process`]

### 24.5 获取进程所有者
[`#ch24.xhtml#get_process_owner`]

### 24.6 获取进程的父进程
[`#ch24.xhtml#get_process_parent`]

### 24.7 调试进程
[`#ch24.xhtml#debug_a_process`]

## 第 25 章：系统服务

### 25.0 简介
[`#ch25.xhtml#introduction-id022`]

### 25.1 列出所有正在运行的服务
[`#ch25.xhtml#list_all_running_services`]

### 25.2 管理正在运行的服务
[`#ch25.xhtml#manage_a_running_service`]

### 25.3 配置服务
[`#ch25.xhtml#configure_a_service`]

## 第 26 章：Active Directory

### 26.0 简介
[`#ch26.xhtml#introduction-id023`]

### 26.1 在本地安装上测试 Active Directory 脚本
[`#ch26.xhtml#test_active_directory_scripts_on_a_local`]




## 26. Active Directory 管理

- 26.2 创建组织单位
- 26.3 获取组织单位的属性
- 26.4 修改组织单位的属性
- 26.5 删除组织单位
- 26.6 获取 Active Directory 容器的子对象
- 26.7 创建用户账户
- 26.8 程序：批量将用户导入 Active Directory
- 26.9 搜索用户账户
- 26.10 获取并列出用户账户的属性
- 26.11 修改用户账户的属性
- 26.12 更改用户密码
- 26.13 创建安全组或通讯组
- 26.14 搜索安全组或通讯组
- 26.15 获取组的属性
- 26.16 查找组的所有者
- 26.17 修改安全组或通讯组的属性
- 26.18 将用户添加到安全组或通讯组
- 26.19 从安全组或通讯组中移除用户
- 26.20 列出用户的组成员身份
- 26.21 列出组的成员
- 26.22 列出组织单位中的用户
- 26.23 搜索计算机账户
- 26.24 获取并列出计算机账户的属性

## 27. 企业计算机管理

- 27.0 简介
- 27.1 将计算机加入域或工作组
- 27.2 将计算机从域中移除
- 27.3 重命名计算机
- 27.4 程序：列出用户的登录或注销脚本
- 27.5 程序：列出计算机的启动或关机脚本
- 27.6 部署基于 PowerShell 的登录脚本
- 27.7 启用或禁用 Windows 防火墙
- 27.8 在 Windows 防火墙中打开或关闭端口
- 27.9 程序：列出所有已安装的软件
- 27.10 卸载应用程序
- 27.11 管理计算机还原点
- 27.12 重启或关闭计算机
- 27.13 确定修补程序是否已安装
- 27.14 管理计算机上的计划任务
- 27.15 检索打印机信息
- 27.16 检索打印机队列统计信息
- 27.17 管理打印机和打印队列
- 27.18 程序：汇总系统信息
- 27.19 续订 DHCP 租约
- 27.20 分配静态 IP 地址
- 27.21 列出计算机的所有 IP 地址
- 27.22 列出网络适配器属性

## 28. CIM 和 Windows 管理规范

- 28.0 简介
- 28.1 访问 Windows 管理规范和 CIM 数据
- 28.2 修改 WMI 或 CIM 实例的属性
- 28.3 在 WMI 实例或类上调用方法
- 28.4 程序：确定 WMI 和 CIM 筛选器可用的属性
- 28.5 搜索用于完成任务的 WMI 或 CIM 类
- 28.6 使用 .NET 执行高级 WMI 任务
- 28.7 将 VBScript WMI 脚本转换为 PowerShell

## 29. 远程处理

- 29.0 简介
- 29.1 查找支持自身远程处理的命令
- 29.2 在计算机上启用 PowerShell 远程处理
- 29.3 启用 SSH 作为 PowerShell 远程处理传输
- 29.4 以交互方式管理远程计算机
- 29.5 在远程计算机上调用命令
- 29.6 断开并重新连接 PowerShell 会话
- 29.7 程序：远程启用 PowerShell 远程处理
- 29.8 程序：在远程计算机上调用 PowerShell 表达式
- 29.9 测试两台计算机之间的连通性
- 29.10 将网络脚本限制为仅针对响应的主机
- 29.11 在计算机上启用远程桌面
- 29.12 配置远程处理的用户权限
- 29.13 启用对工作组计算机的远程处理
- 29.14 从远程计算机隐式调用命令
- 29.15 创建具有完全网络访问权限的会话
- 29.16 将变量传递到远程会话
- 29.17 在远程计算机上管理和编辑文件
- 29.18 配置高级远程处理配额和选项
- 29.19 在多台计算机上调用命令
- 29.20 在远程计算机上运行本地脚本





## 29.21. 确定脚本是否在远程计算机上运行
#ch29.xhtml#determine_script_running_remote_computer

## 30. 事务
#ch30.xhtml#chapter_transactions

### 30.0. 简介
#ch30.xhtml#ch31_id2325811

### 30.1. 安全地试验事务
#ch30.xhtml#safely_experiment_with_transactions

### 30.2. 更改事务中的错误恢复行为
#ch30.xhtml#change_rollback_preference

## 31. 事件处理
#ch31.xhtml#event_handling

### 31.0. 简介
#ch31.xhtml#event-handling-intro

### 31.1. 响应自动生成的事件
#ch31.xhtml#respond_generated_events

### 31.2. 创建并响应自定义事件
#ch31.xhtml#respond_custom_events

### 31.3. 创建临时事件订阅
#ch31.xhtml#create-temporary-event-subscription

### 31.4. 从远程计算机转发事件
#ch31.xhtml#forward_event_remote_machine

### 31.5. 调查内部事件操作状态
#ch31.xhtml#investigate_action_state

### 31.6. 将脚本块用作 .NET 委托或事件处理程序
#ch31.xhtml#scriptblock_as_delegate

## 参考资料
#part05.xhtml#part5

## PowerShell 语言与环境
#app01.xhtml#powershell_language_and_environment

### 命令与表达式
#app01.xhtml#commands_and_expressions

### 注释
#app01.xhtml#comments

### 帮助注释
#app01.xhtml#help_comments

### 变量
#app01.xhtml#variables-id001

### 布尔值
#app01.xhtml#booleans

### 字符串
#app01.xhtml#strings

### 数字
#app01.xhtml#numbers

### 数组与列表
#app01.xhtml#arrays_and_lists

### 哈希表（关联数组）
#app01.xhtml#hashtables_associative_arrays

### XML
#app01.xhtml#xml

### 简单运算符
#app01.xhtml#simple_operators

### 比较运算符
#app01.xhtml#comparison_operators

### 条件语句
#app01.xhtml#conditional_statements

### 循环语句
#app01.xhtml#looping_statements

### 使用 .NET Framework
#app01.xhtml#working_with_the_net_framework

### 编写脚本，重用功能
#app01.xhtml#writing_scripts_reusing_functionality

### 错误管理
#app01.xhtml#managing_errors

### 格式化输出
#app01.xhtml#formatting_output

### 捕获输出
#app01.xhtml#capturing_output

### 常见自定义点
#app01.xhtml#common_customization_points

## 正则表达式参考
#app02.xhtml#regular_expression_reference

## XPath 快速参考
#app03.xhtml#xpath_reference

## .NET 字符串格式化
#app04.xhtml#net_string_formatting

### 字符串格式化语法
#app04.xhtml#string_formatting_syntax

### 标准数字格式字符串
#app04.xhtml#standard_numeric_format_strings

### 自定义数字格式字符串
#app04.xhtml#custom_numeric_format_strings

## .NET DateTime 格式化
#app05.xhtml#net_datetime_formatting

### 自定义 DateTime 格式字符串
#app05.xhtml#custom_datetime_format_strings

## 精选 .NET 类及其用法
#app06.xhtml#selected_net_classes_and_their_uses

## WMI 参考
#app07.xhtml#wmi_reference

## 精选 COM 对象及其用法
#app08.xhtml#selected_com_objects_and_their_uses

## 精选事件及其用法
#app09.xhtml#selected_events_and_their_uses

## 标准 PowerShell 动词
#app10.xhtml#standard_powershell_verbs

## 索引
#ix01.xhtml#idm45279966977160

