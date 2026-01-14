# GitHub Pages 兼容性指南

## 问题原因

GitHub Pages 和本地环境的主要差异：

1. **大小写敏感性**：GitHub 服务器（Linux）对文件名大小写敏感，Windows 不敏感
2. **Jekyll 处理**：GitHub Pages 默认使用 Jekyll，可能影响某些文件
3. **路径问题**：相对路径在不同环境下可能表现不同
4. **缓存问题**：浏览器和 GitHub 服务器可能有缓存

## 已实施的解决方案

### 1. `.nojekyll` 文件
- ✅ 已创建，禁用 Jekyll 处理
- 确保所有文件按原样部署，不被 Jekyll 处理

### 2. 文件路径验证
已检查并确认所有文件路径大小写一致：
- ✅ `styles.css` → 引用正确
- ✅ `avatar.jpg` → 引用正确
- ✅ `ARAIAG.gif` → 引用正确
- ✅ `SAMR.gif` → 引用正确
- ✅ `APH.gif` → 引用正确

### 3. GitHub Actions 工作流
- ✅ 已创建 `.github/workflows/pages.yml`
- 确保每次推送时自动部署
- 使用最新的 GitHub Pages Actions

## 部署检查清单

在推送到 GitHub 之前，请确认：

### 文件完整性
- [ ] 所有图片文件已上传（avatar.jpg, *.gif）
- [ ] styles.css 文件存在
- [ ] index.html 文件存在
- [ ] `.nojekyll` 文件存在

### 路径检查
- [ ] 所有 `src=` 和 `href=` 路径使用相对路径
- [ ] 文件名大小写与引用完全一致
- [ ] 没有使用 Windows 特定的路径分隔符（`\`）

### 测试建议

1. **本地测试**：
   ```bash
   # 使用 Python 简单服务器测试（模拟 GitHub Pages 环境）
   python -m http.server 8000
   # 然后访问 http://localhost:8000
   ```

2. **GitHub 测试**：
   - 推送后等待 1-2 分钟
   - 清除浏览器缓存（Ctrl+Shift+Delete）
   - 使用无痕模式访问
   - 检查浏览器控制台是否有 404 错误

## 常见问题排查

### 问题 1: 图片不显示
**可能原因**：
- 文件名大小写不匹配
- 文件未上传到 GitHub
- 路径错误

**解决方法**：
1. 检查 GitHub 仓库中的实际文件名
2. 确保 HTML 中的引用与文件名完全一致（包括大小写）
3. 使用相对路径，不要使用绝对路径

### 问题 2: CSS 样式不生效
**可能原因**：
- styles.css 文件路径错误
- 文件未上传
- 浏览器缓存

**解决方法**：
1. 检查 `<link rel="stylesheet" href="styles.css">` 路径
2. 确认文件在仓库根目录
3. 清除浏览器缓存

### 问题 3: 气泡文字不显示
**可能原因**：
- CSS 定位问题（已在 styles.css 中修复）
- JavaScript 执行错误

**解决方法**：
1. 检查浏览器控制台是否有 JavaScript 错误
2. 确认 `.hockey-easter-egg` 有 `position: relative`
3. 确认 `.speech-bubble` 的 z-index 足够高

## 部署命令

```bash
# 1. 检查文件状态
git status

# 2. 添加所有文件
git add .

# 3. 提交更改
git commit -m "Update website: fix compatibility issues"

# 4. 推送到 GitHub
git push origin main

# 5. 等待 1-2 分钟后访问
# https://yourusername.github.io
```

## 验证部署

部署后，检查以下内容：

1. **页面加载**：网站能正常打开
2. **样式**：CSS 样式正确应用
3. **图片**：所有图片正常显示
4. **交互**：气泡文字能正常显示
5. **响应式**：在不同设备上测试

## 强制刷新缓存

如果更改后看不到效果：

1. **浏览器**：Ctrl+Shift+R（Windows）或 Cmd+Shift+R（Mac）
2. **GitHub**：在仓库 Settings → Pages 中重新保存设置
3. **等待**：GitHub Pages 更新可能需要几分钟

## 技术支持

如果问题仍然存在：
1. 检查 GitHub 仓库的文件列表
2. 查看浏览器开发者工具的控制台和网络标签
3. 确认 GitHub Pages 设置正确（Settings → Pages）

