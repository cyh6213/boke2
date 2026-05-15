# 语雀知识库浏览技能

## 技能描述
用于浏览语雀（yuque.com）知识库中的文档和项目内容。

## 使用场景
当用户让你查看语雀上的某个页面、项目或知识库内容时使用此技能。

## 操作流程

### 1. 导航到目标页面
```
browser_navigate - 导航到语雀知识库页面
参数: url - 目标URL
```

### 2. 获取页面快照并分析
```
browser_snapshot - 获取当前页面所有可交互元素
```
查看 snapshot 结果中的：
- `role: link` - 可点击的链接
- `role: button` - 可点击的按钮
- `ref` - 元素的引用ID，用于后续点击
- `name` - 元素名称/标题

### 3. 处理覆盖层问题
如果点击时出现 "Click target intercepted" 错误，说明有覆盖层阻挡：
```
browser_press_key - 按 Escape 关闭覆盖层
参数: key - "Escape"
```

### 4. 处理侧边栏目录
#### 如果目录被折叠
- 寻找类似 "目录"、"展开" 等按钮点击展开

#### 如果目录需要滑动
- 使用 `browser_scroll` 滑动侧边栏
- 参数: selector - 侧边栏的选择器（如 "#aside"）
- 参数: direction - "up" 或 "down"
- 参数: amount - 滑动像素

### 5. 点击目标链接
```
browser_click - 点击页面元素
参数:
  - ref - 元素的引用ID
  - take_screenshot_afterwards - true（可选，便于记录）
```

### 6. 记录内容
- 记录页面的 URL 和标题
- 记录关键内容模块
- 按顺序浏览所有相关子页面

## 常见问题

### Q: 点击链接失败，显示 "Element not found"
A: 元素引用已过期，需要重新执行 `browser_snapshot` 获取最新的元素引用

### Q: 侧边栏找不到目标分类
A: 尝试：
1. 先按 Escape 关闭可能存在的覆盖层
2. 使用 browser_scroll 滑动侧边栏查找
3. 检查页面顶部是否有"目录"按钮可点击

### Q: 点击后页面没反应
A: 检查是否需要等待页面加载，可再次执行 `browser_snapshot` 确认

## 示例：浏览 RAG 项目

```
# 1. 导航到知识库首页
browser_navigate - url: "https://www.yuque.com/用户名/知识库ID"

# 2. 获取快照，找到 agent 分类链接
browser_snapshot

# 3. 点击 agent 分类
browser_click - ref: "e39"（假设这是agent链接的ref）

# 4. 获取快照，找到 RAG 子目录
browser_snapshot

# 5. 点击 RAG 链接
browser_click - ref: "e60"

# 6. 获取快照，查看 RAG 下的所有子文件
browser_snapshot
# 发现以下子文件：
# - 记忆
# - 三次改进
# - Agent项目
# - 文件上传与解析
# - 文本处理、混合检索
# - LLM交互
# - 权限/登陆控制

# 7. 依次点击每个子文件查看内容
browser_click - ref: "子文件链接的ref"
browser_snapshot

# 8. 重复直到查看完所有需要的文件
```

## 注意事项
- 语雀页面经常有覆盖层（overlay），遇到点击失败先按 Escape
- 侧边栏目录可能需要滑动才能看到更多内容
- 每次页面变化后都需要重新获取快照以获取最新的元素引用
- 语雀的链接格式通常是：https://www.yuque.com/用户名/知识库ID/页面ID