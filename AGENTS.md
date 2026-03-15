# 单商户国内购物商城 - 需求拆解文档

## 产品概述

- **产品类型**: 单商户B2C自营电商系统（含用户端前台 + 后台管理端）
- **场景类型**: prototype - app
- **目标用户**: 国内普通消费者（买家）、商城运营管理员
- **核心价值**: 提供完整的在线购物闭环体验，支持商品浏览、购物车管理、优惠码抵扣、在线支付、订单跟踪及后台运营管理
- **界面语言**: zh-CN（中文）
- **主题偏好**: light（浅色主题，简洁清爽风格）
- **导航模式**: 路径导航（前台Topbar + 后台Sidebar组合）

---

## 页面结构总览

### 用户端前台（Consumer Frontend）

| 页面名称 | 文件名 | 路由 | 页面类型 | 入口来源 |
|---------|-------|------|---------|---------|
| 首页 | `HomePage.tsx` | `/` | 一级 | 导航 |
| 商品分类/搜索 | `CategoryPage.tsx` | `/category/:id?` | 一级 | 导航、首页分类入口 |
| 商品详情 | `ProductDetailPage.tsx` | `/product/:id` | 二级 | 首页/分类页 → 商品卡片点击 |
| 购物车 | `CartPage.tsx` | `/cart` | 一级 | 导航、商品详情页 |
| 订单确认 | `CheckoutPage.tsx` | `/checkout` | 二级 | 购物车 → 去结算 |
| 订单列表 | `OrdersPage.tsx` | `/orders` | 一级 | 导航、个人中心 |
| 订单详情 | `OrderDetailPage.tsx` | `/orders/:id` | 二级 | 订单列表 → 订单项点击 |
| 个人中心 | `ProfilePage.tsx` | `/profile` | 一级 | 导航 |
| 登录/注册 | `LoginPage.tsx` | `/login` | 特殊 | 未登录状态自动跳转 |

### 后台管理端（Admin Backend）

| 页面名称 | 文件名 | 路由 | 页面类型 | 入口来源 |
|---------|-------|------|---------|---------|
| 管理首页 | `AdminDashboard.tsx` | `/admin` | 一级 | 导航 |
| 商品管理 | `AdminProducts.tsx` | `/admin/products` | 一级 | 导航 |
| 商品编辑 | `AdminProductEdit.tsx` | `/admin/products/:id?` | 二级 | 商品管理 → 新建/编辑按钮 |
| 订单管理 | `AdminOrders.tsx` | `/admin/orders` | 一级 | 导航 |
| 订单详情 | `AdminOrderDetail.tsx` | `/admin/orders/:id` | 二级 | 订单管理 → 查看详情 |
| 会员管理 | `AdminMembers.tsx` | `/admin/members` | 一级 | 导航 |
| 营销中心 | `AdminMarketing.tsx` | `/admin/marketing` | 一级 | 导航 |
| 统计分析 | `AdminStats.tsx` | `/admin/stats` | 一级 | 导航 |
| 系统设置 | `AdminSettings.tsx` | `/admin/settings` | 一级 | 导航 |

> **页面类型说明**：
> - **一级页面**：出现在导航中，用户可直接访问
> - **二级页面**：不在导航中，从一级页面跳转进入

---

## 导航配置

### 用户端前台导航（Topbar）

- **导航布局**: Topbar（顶部固定，包含Logo、搜索、主要入口）
- **导航项**:
  | 导航文字 | 路由 | 说明 |
  |---------|------|------|
  | 首页 | `/` | 网站Logo旁主入口 |
  | 分类 | `/category` | 商品分类浏览 |
  | 购物车 | `/cart` | 显示购物车商品数量角标 |
  | 我的 | `/profile` | 个人中心（已登录）/ 登录（未登录） |

### 后台管理端导航（Sidebar）

- **导航布局**: Sidebar（左侧固定侧边栏）
- **导航项**:
  | 导航文字 | 路由 | 图标建议 |
  |---------|------|---------|
  | 概览 | `/admin` | Dashboard |
  | 商品管理 | `/admin/products` | ShoppingBag |
  | 订单管理 | `/admin/orders` | ClipboardList |
  | 会员管理 | `/admin/members` | Users |
  | 营销中心 | `/admin/marketing` | Tag |
  | 统计分析 | `/admin/stats` | BarChart3 |
  | 系统设置 | `/admin/settings` | Settings |

---

## 功能列表

### 用户端前台

- **页面**: 首页 (`HomePage`)
  - **页面目标**: 展示商城核心内容，引导用户进入商品浏览
  - **功能点**:
    - 轮播图展示（后台配置，可点击跳转）
    - 分类快捷入口（图标+名称，点击进入分类页）
    - 推荐商品区（后台配置权重排序，最多20个）
    - 新品上市区（按上架时间倒序）
    - 底部加载更多/无限滚动分页

- **页面**: 商品分类/搜索 (`CategoryPage`)
  - **页面目标**: 支持按分类浏览和关键词搜索商品
  - **功能点**:
    - 多级分类展示（左侧一级，右侧二级+三级）
    - 商品列表展示（图片、名称、价格、销量）
    - 排序切换（综合、销量、价格、上架时间）
    - 筛选功能（价格区间输入、品牌筛选）
    - 搜索历史记录（最近10条，可清空）
    - 热门搜索关键词（后台配置）

- **页面**: 商品详情 (`ProductDetailPage`)
  - **页面目标**: 展示商品完整信息，支持SKU选择和购买操作
  - **功能点**:
    - 主图轮播（多图+视频支持）
    - 商品基础信息（名称、价格、划线价、销量、评价数）
    - SKU选择器（规格联动，实时显示库存价格）
    - 商品参数表格展示
    - 图文详情（富文本内容）
    - 用户评价区（最新3条+查看更多入口）
    - 底部固定操作栏（收藏、加入购物车、立即购买）

- **页面**: 购物车 (`CartPage`)
  - **页面目标**: 管理用户选购的商品，支持批量操作和结算
  - **功能点**:
    - 商品列表（图片、名称、规格、单价、数量、小计）
    - 数量修改（加减按钮+直接输入）
    - 单个/批量删除商品
    - 失效商品标记（下架/库存不足置灰提示）
    - 全选/取消全选功能
    - 底部价格汇总（商品总价、运费估算、选中数量）
    - 去结算按钮（携带选中商品跳转订单确认）

- **页面**: 订单确认 (`CheckoutPage`)
  - **页面目标**: 确认订单信息，完成支付前的最终校验
  - **功能点**:
    - 收货地址选择（默认地址展示，支持切换/新增）
    - 配送方式选择（根据运费模板计算运费）
    - 支付方式选择（微信支付、支付宝）
    - 优惠码输入与验证（实时校验并显示优惠金额）
    - 发票信息填写（个人/公司抬头、税号）
    - 订单备注输入（最多100字）
    - 金额明细（商品总价、运费、优惠、实付总额）
    - 提交订单按钮（生成订单并跳转支付）

- **页面**: 订单列表 (`OrdersPage`)
  - **页面目标**: 查看历史订单，按状态筛选和管理
  - **功能点**:
    - 状态标签筛选（全部、待付款、待发货、待收货、待评价、售后）
    - 订单卡片展示（订单号、时间、商品缩略图、金额、状态）
    - 分页加载
    - 状态操作按钮（取消订单、去支付、提醒发货、确认收货、去评价）

- **页面**: 订单详情 (`OrderDetailPage`)
  - **页面目标**: 展示订单完整信息，支持物流查询和售后
  - **功能点**:
    - 订单状态跟踪
    - 收货信息展示
    - 物流信息展示（快递公司、运单号、轨迹查询）
    - 商品明细列表
    - 支付与优惠码信息
    - 发票信息展示
    - 操作按钮（根据状态：取消、确认收货、申请售后、再次购买）

- **页面**: 个人中心 (`ProfilePage`)
  - **页面目标**: 管理用户个人信息和收货地址
  - **功能点**:
    - 头像与昵称编辑
    - 绑定手机修改（需原手机验证）
    - 收货地址管理（增删改查、省市区联动、默认地址设置）
    - 密码修改（原密码验证+复杂度校验）
    - 我的收藏（收藏夹商品列表，支持取消收藏、加入购物车）

- **页面**: 登录/注册 (`LoginPage`)
  - **页面目标**: 用户身份认证与账户创建
  - **功能点**:
    - 手机号+验证码登录（60秒倒计时重发）
    - 手机号+密码登录（记住密码选项）
    - 用户协议与隐私政策勾选确认

### 后台管理端

- **页面**: 管理首页 (`AdminDashboard`)
  - **页面目标**: 展示商城核心运营数据概览
  - **功能点**:
    - 今日/昨日/本月销售数据卡片（销售额、订单数、客单价、转化率）
    - 销售额趋势图（折线图，支持日/月维度）
    - 订单量趋势图
    - 环比/同比数据展示
    - 待处理事项提醒（待发货订单数、待处理售后数）

- **页面**: 商品管理 (`AdminProducts`)
  - **页面目标**: 管理商城商品上下架与基本信息
  - **功能点**:
    - 商品列表（分类、状态、名称搜索筛选）
    - 批量操作（上架、下架、删除）
    - 商品状态标签（已上架、已下架、定时上架）
    - 快捷编辑入口

- **页面**: 商品编辑 (`AdminProductEdit`)
  - **页面目标**: 创建新商品或编辑现有商品信息
  - **功能点**:
    - 基本信息编辑（名称、副标题、关键词、分类、品牌）
    - 主图上传管理（最多10张，拖拽排序）
    - 视频上传（可选）
    - SKU管理（多规格设置，每个SKU独立图片、价格、库存、货号）
    - 价格库存设置（定价、市场价、库存预警）
    - 商品参数设置（自定义键值对）
    - 图文详情编辑（富文本）
    - 上架时间设置（立即/定时）

- **页面**: 订单管理 (`AdminOrders`)
  - **页面目标**: 处理用户订单，完成发货等操作
  - **功能点**:
    - 订单列表（多维度搜索：订单号、手机号、收货人、时间、状态）
    - 订单导出（Excel格式）
    - 快速筛选（待付款、待发货、待收货等状态）
    - 订单操作（发货：选择物流+填运单号、修改价格、取消订单、添加备注）

- **页面**: 订单详情 (`AdminOrderDetail`)
  - **页面目标**: 查看订单完整信息，处理售后
  - **功能点**:
    - 订单完整信息展示（收货、商品、支付、优惠码、发票）
    - 操作日志查看
    - 售后申请处理（同意/拒绝退款，填写原因）
    - 发货单/快递单打印（可选）

- **页面**: 会员管理 (`AdminMembers`)
  - **页面目标**: 查看和管理注册用户
  - **功能点**:
    - 会员列表（昵称、手机号搜索，展示注册时间、订单数、累计消费）
    - 会员详情查看（收货地址、历史订单、优惠码使用记录）
    - 黑名单管理（加入/移除黑名单，禁止登录下单）

- **页面**: 营销中心 (`AdminMarketing`)
  - **页面目标**: 管理优惠码活动
  - **功能点**:
    - 优惠码列表（搜索、筛选：状态、有效期）
    - 优惠码创建/编辑（名称、代码、折扣类型、折扣值、上限、门槛、有效期、限用次数、适用范围）
    - 数据统计（使用次数、优惠金额、带来订单数）
    - 启用/禁用/删除操作

- **页面**: 统计分析 (`AdminStats`)
  - **页面目标**: 深度数据分析与经营决策支持
  - **功能点**:
    - 销售概览（时间段选择、趋势图）
    - 商品排行（热销TOP10、滞销TOP10）
    - 分类销售占比（饼图）
    - 用户分析（新增用户、活跃用户、留存率）
    - 财务对账（支付流水、退款、实收统计，支持导出）

- **页面**: 系统设置 (`AdminSettings`)
  - **页面目标**: 配置商城基础信息、支付与物流
  - **功能点**:
    - 商城基础配置（名称、LOGO、客服电话、SEO信息、底部备案链接）
    - 支付配置（微信支付：AppID、商户号、密钥、证书；支付宝：AppID、公钥、私钥；沙箱开关）
    - 物流配置（快递公司维护、运费模板设置：按件/按重量计费、地区差异化运费）
    - 发票配置（开关、类型限制）
    - 权限管理（管理员账号、角色权限分配）
    - 操作日志查询

---

## 数据共享配置

| 存储键名 | 数据说明 | 使用页面 |
|---------|---------|---------|
| `__global_shop_user` | 当前登录用户信息，类型为 `IUser` | 所有页面（判断登录状态） |
| `__global_shop_cart` | 购物车数据列表，类型为 `ICartItem[]` | 商品详情、购物车、订单确认 |
| `__global_shop_currentOrder` | 当前正在确认的订单商品，类型为 `ICartItem[]` | 购物车 → 订单确认页传递 |
| `__global_shop_selectedAddress` | 当前选中的收货地址，类型为 `IAddress` | 订单确认、订单详情 |
| `__global_shop_searchKeyword` | 当前搜索关键词，类型为 `string` | 分类/搜索页 |

```ts
interface IUser {
  id: string;
  nickname: string;
  avatar?: string;
  phone: string;
  isLogin: boolean;
}

interface ICartItem {
  id: string;
  productId: string;
  skuId: string;
  name: string;
  image: string;
  specs: Record<string, string>;
  price: number;
  quantity: number;
  stock: number;
  selected: boolean;
  valid: boolean; // 是否有效（未下架、有库存）
}

interface IAddress {
  id: string;
  name: string;
  phone: string;
  province: string;
  city: string;
  district: string;
  detail: string;
  isDefault: boolean;
}
```

---

## 合规与视觉要求摘要

### 视觉风格要求
- **整体风格**: 简洁、清爽、专业，留白充足
- **圆角规范**: 按钮、卡片统一圆角8px
- **图标风格**: 线性图标（FontAwesome风格）
- **响应式**: 支持移动端适配
- **品牌色**: 主色调待定（建议红色#FF4400或蓝色#007AFF）

### 合规要求
- **ICP备案**: 底部展示备案号，链接至 `https://beian.miit.gov.cn`
- **隐私政策**: 注册时弹窗同意，底部常驻链接
- **用户协议**: 注册时弹窗同意，底部常驻链接
- **营业执照**: 底部可选展示
- **版权信息**: `© 2026 XXX商城 版权所有`

### 页面布局统一要求
- **头部统一**: Logo(200x60px) + 搜索框 + 用户入口 + 购物车
- **底部统一**: 关于我们、帮助中心、联系方式、ICP备案号、客服电话、版权信息

-------

# UI 设计指南

> **场景类型**: `prototype - app`（应用架构设计）
> **确认检查**: 本指南适用于多页面电商应用系统（前台+后台），包含复杂导航结构和表单交互。

> ℹ️ Section 1-2 为设计意图与决策上下文。Code agent 实现时以 Section 3 及之后的具体参数为准。

## 1. Design Archetype (设计原型)

### 1.1 内容理解
- **目标用户**: 
  - 前台：国内普通消费者（18-50岁，熟悉移动端购物，期望快速完成交易）
  - 后台：商城运营管理员（需高效处理商品、订单、数据，注重操作效率）
- **核心目的**: 
  - 前台：建立信任感，引导顺畅完成浏览-加购-支付闭环
  - 后台：信息密度高，操作路径清晰，支持批量处理
- **期望情绪**: 
  - 前台：信任（正规商城）、便捷（一目了然）、愉悦（购物满足感）
  - 后台：高效、专业、可控
- **需避免的感受**: 
  - 前台：焦虑（复杂的优惠规则）、困惑（找不到购物车）、廉价感（杂乱布局）
  - 后台：混乱（信息过载）、低效（过多跳转）

### 1.2 设计语言
- **Aesthetic Direction**: 现代极简电商风格，通过充足的留白和清晰的信息层级建立专业感，红色主色传递促销活力与行动召唤
- **Visual Signature**: 
  1. 统一的 8px 圆角体系（从按钮到卡片）
  2. 高对比红色主按钮（#FF4400）作为行动锚点
  3. 线性图标系统（简洁、统一、不喧宾夺主）
  4. 充足的卡片留白（p-5 ~ p-6）
  5. 清晰的电商信息层级（价格突出、原价划线、库存状态明确）
- **Emotional Tone**: 清爽可信（前台）+ 高效专业（后台）
- **Design Style**: Rounded 圆润几何 — 电商场景需降低认知摩擦，统一 8px 圆角创造柔和友好的购物体验，与线性图标搭配保持简洁专业
- **Application Type**: E-commerce App（前台 Landing/商城风格 + 后台管理系统）

## 2. Design Principles (设计理念)

1. **清晰的信息层级**: 价格、标题、操作按钮通过字号、字重、颜色建立明确视觉层级，用户3秒内抓住关键信息
2. **一致的交互语言**: 所有可点击元素（按钮、链接、卡片）有统一的 hover/active 反馈，降低学习成本
3. **充足的呼吸空间**: 商品卡片、表单元素周围保持充足留白，避免信息拥挤带来的焦虑感
4. **可信的视觉信号**: 通过正规备案信息展示、安全的支付流程提示、清晰的售后政策建立信任
5. **响应式无缝体验**: 移动端与PC端保持一致的视觉语言，确保跨设备购物体验连贯

## 3. Color System (色彩系统)

> **⚠️ App 场景配色规则**：本设计基于电商场景自主推导完整配色体系，未使用预设配色方案。
> **配色设计理由**：选择橙红色系（#FF4400）作为主色，符合国内电商用户认知（促销、行动、 urgency），同时避免过于艳丽的正红，保持专业感；背景使用极浅暖灰，营造温和不刺眼的购物环境。

### 3.1 主题颜色

| 角色 | CSS 变量 | Tailwind Class | HSL 值 | 设计说明 |
|-----|---------|----------------|--------|---------|
| bg | `--background` | `bg-background` | `hsl(0 0% 98%)` | 极浅暖灰，减少纯白刺眼感，营造柔和购物环境 |
| surface | `--card` | `bg-card` | `hsl(0 0% 100%)` | 纯白卡片背景，与页面背景形成层次 |
| text | `--foreground` | `text-foreground` | `hsl(0 0% 13%)` | 近黑文字，确保可读性，避免纯黑过于沉重 |
| textMuted | `--muted-foreground` | `text-muted-foreground` | `hsl(0 0% 45%)` | 中灰，用于辅助信息、原价、占位符 |
| primary | `--primary` | `bg-primary` | `hsl(15 100% 50%)` | 电商橙红（#FF4400），行动召唤，用于主按钮、价格、重点标记 |
| primary-foreground | `--primary-foreground` | `text-primary-foreground` | `hsl(0 0% 100%)` | 白色，用于主色背景上的文字 |
| accent | `--accent` | `bg-accent` | `hsl(15 100% 97%)` | 极浅橙红，用于 hover/focus 状态背景、幽灵按钮悬停 |
| accent-foreground | `--accent-foreground` | `text-accent-foreground` | `hsl(15 100% 40%)` | 深橙红，用于 accent 背景上的文字 |
| border | `--border` | `border-border` | `hsl(0 0% 90%)` | 浅灰边框，分割元素但不突兀 |
| muted | `--muted` | `bg-muted` | `hsl(0 0% 96%)` | 更浅的灰，用于禁用态背景、骨架屏 |
| destructive | `--destructive` | `bg-destructive` | `hsl(0 72% 51%)` | 错误/删除操作红，与主色区分 |

> **Color Token 语义速查（供 code agent 参考）**:
> - `primary` → 主行动：立即购买按钮、加入购物车、结算、支付，以及商品现价、促销标签
> - `accent` → 状态反馈：按钮 hover 背景、输入框 focus 边框、下拉菜单选中背景、骨架屏占位
> - `muted` → 静态非交互：禁用按钮、已售罄标签、分割线、表头背景
> - `textMuted` → 辅助信息：商品原价（划线价）、库存提示、时间戳、次要说明文字

### 3.2 Sidebar 颜色（后台管理端）

| 角色 | CSS 变量 | Tailwind Class | HSL 值 | 设计说明 |
|-----|---------|----------------|--------|---------|
| sidebar | `--sidebar` | `bg-sidebar` | `hsl(0 0% 100%)` | 侧边栏背景，与卡片一致，保持清爽 |
| sidebar-foreground | `--sidebar-foreground` | `text-sidebar-foreground` | `hsl(0 0% 13%)` | 导航文字颜色 |
| sidebar-primary | `--sidebar-primary` | `bg-sidebar-primary` | `hsl(15 100% 97%)` | 激活态菜单背景，使用 accent 色 |
| sidebar-primary-foreground | `--sidebar-primary-foreground` | `text-sidebar-primary-foreground` | `hsl(15 100% 50%)` | 激活态菜单文字，使用主色 |
| sidebar-accent | `--sidebar-accent` | `bg-sidebar-accent` | `hsl(0 0% 96%)` | Hover 态背景，浅灰 |
| sidebar-accent-foreground | `--sidebar-accent-foreground` | `text-sidebar-accent-foreground` | `hsl(0 0% 13%)` | Hover 态文字 |
| sidebar-border | `--sidebar-border` | `border-sidebar-border` | `hsl(0 0% 90%)` | 侧边栏右侧边框，分隔内容区 |
| sidebar-ring | `--sidebar-ring` | `ring-sidebar-ring` | `hsl(15 100% 50%)` | 聚焦环颜色，与主色一致 |

### 3.3 语义颜色（状态反馈）

| 用途 | 角色 | HSL 值 | 使用场景 |
|-----|-----|--------|---------|
| 成功/上升 | `--success` | `hsl(142 71% 45%)` | 支付成功、库存充足、销量上升、正向提示 |
| 警告/待处理 | `--warning` | `hsl(38 92% 50%)` | 待付款、库存紧张、等待发货、警告提示 |
| 错误/失败 | `--destructive` | `hsl(0 72% 51%)` | 支付失败、库存不足、删除操作、错误提示 |
| 信息/中性 | `--info` | `hsl(221 83% 53%)` | 物流信息、帮助提示、中性通知 |

## 4. Typography (字体排版)

- **Heading**: Inter, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "PingFang SC", "Microsoft YaHei", sans-serif
- **Body**: Inter, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "PingFang SC", "Microsoft YaHei", sans-serif
- **字体导入**: 
  ```css
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
  ```

**字号规范**：

| 层级 | 尺寸 | 字重 | 行高 | 用途 |
|-----|-----|-----|-----|-----|
| 页面主标题 | `text-2xl` (24px) | `font-bold` (700) | `leading-tight` | 后台页面标题、订单确认页标题 |
| 卡片标题 | `text-lg` (18px) | `font-semibold` (600) | `leading-snug` | 商品名称、区块标题 |
| 正文 | `text-base` (16px) | `font-normal` (400) | `leading-relaxed` | 描述文字、表单标签 |
| 辅助文字 | `text-sm` (14px) | `font-normal` (400) | `leading-normal` | 时间戳、次要信息、提示 |
| 价格大数字 | `text-2xl` (24px) | `font-bold` (700) | `leading-none` | 商品现价、订单总额 |
| 价格小数字 | `text-base` (16px) | `font-medium` (500) | `leading-none` | 购物车小计、运费 |
| 划线价 | `text-sm` (14px) | `font-normal` (400) | `leading-none` | 原价，配合 `line-through text-muted-foreground` |

## 5. Global Layout Structure (全局布局结构)

### 5.1 前台（用户端）布局

**Standard Content Zone**:
- **Maximum Width**: `max-w-6xl` (1152px)
- **Padding**: `px-4 md:px-6`（移动端紧凑，桌面端舒适）
- **Alignment**: `mx-auto`
- **Vertical Spacing**: `gap-6` / `space-y-6`（内容区块间距）

**Topbar 导航**:
- **定位**: `sticky top-0 z-50`（滚动时固定）
- **背景**: `bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60`（毛玻璃效果）
- **高度**: `h-16` (64px)
- **阴影**: `shadow-sm`（滚动后显示）
- **结构**: 
  - 左侧：Logo（200x60px）+ 主导航链接（首页、分类）
  - 中间：搜索框（`max-w-md`，圆角full）
  - 右侧：购物车图标（带数量角标）+ 用户入口（登录/头像）

**Footer 统一底部**:
- **背景**: `bg-muted` 或 `bg-card`（与页面区分）
- **内边距**: `py-12 px-4 md:px-6`
- **内容**: 
  - 上部：关于我们、帮助中心、联系我们链接（flex 横向排列）
  - 中部：客服电话、工作时间
  - 下部：ICP备案号（带链接）、版权信息 `© 2026 XXX商城 版权所有`
- **文字**: `text-sm text-muted-foreground`，备案链接 `hover:text-primary`

### 5.2 后台（管理端）布局

**Sidebar 导航**:
- **定位**: `fixed left-0 top-0 z-40 h-full`
- **宽度**: `w-64` (256px)
- **背景**: `bg-sidebar`
- **边框**: `border-r border-sidebar-border`
- **结构**:
  - 顶部：后台 Logo（缩小版，160x48px）
  - 中部：导航菜单（图标 + 文字，垂直排列）
  - 底部：管理员信息 + 退出按钮

**Content Area**:
- **左边距**: `lg:ml-64`（为 sidebar 腾出空间）
- **Padding**: `p-4 md:p-6 lg:p-8`
- **Maximum Width**: 无限制（管理系统通常需要全宽展示表格），内容区使用 `max-w-[1400px]` 防止过宽难以阅读
- **背景**: `bg-background`（与前台一致，保持统一性）

### 5.3 页面模式设计

**商品列表页（前台/后台共用逻辑）**:
- **布局**: Grid 网格，`grid-cols-2 md:grid-cols-4 lg:grid-cols-5`（前台），`grid-cols-1 md:grid-cols-2 lg:grid-cols-4`（后台卡片式）
- **卡片结构**: 图片（aspect-square）+ 信息区（p-3）
- **间距**: `gap-4 md:gap-6`

**表单页面（后台商品编辑/设置）**:
- **布局**: 单栏 `max-w-2xl` 或双栏（左侧表单，右侧预览/辅助信息）
- **分区**: 使用卡片分割不同模块（基本信息、SKU管理、图文详情）
- **内边距**: 每个模块 `p-6`

**订单确认/支付页（前台）**:
- **布局**: 双栏（桌面端）：左侧地址+商品+支付信息（`col-span-2`），右侧金额汇总（`col-span-1`，sticky）
- **移动端**: 单栏堆叠，汇总信息固定底部

## 6. Visual Effects & Motion (视觉效果与动效)

- **圆角**: 
  - 按钮/输入框: `rounded-lg` (8px) — 需求明确指定
  - 卡片: `rounded-lg` (8px)
  - 标签/徽章: `rounded-full` (pill)
  - 头像: `rounded-full`
  - 模态框: `rounded-xl` (12px)

- **阴影**:
  - 卡片默认: `shadow-sm`
  - 卡片 Hover: `shadow-md` + `transition-shadow duration-200`
  - 悬浮按钮（客服）: `shadow-lg`
  - Topbar 固定后: `shadow-sm`

- **动效规范**:
  - **缓动函数**: `cubic-bezier(0.4, 0, 0.2, 1)` (ease-out)
  - **时长**: 
    - 微交互（hover）: `duration-200`
    - 页面切换: `duration-300`
    - 模态框/抽屉: `duration-300`
  - **关键动效**:
    1. 按钮 Hover: `scale-[1.02]` + 背景色变化（primary → primary/90）
    2. 卡片 Hover: `translate-y-[-2px]` + shadow 加深
    3. 加入购物车: 按钮文字短暂变为 "已加入" + 购物车图标跳动动画
    4. 页面加载: Skeleton 骨架屏 `animate-pulse`（使用 muted 色）

## 7. Components (组件指南)

### Buttons

| 类型 | 背景 | 文字 | 边框 | Hover | 用途 |
|-----|-----|-----|-----|-------|-----|
| **Primary** | `bg-primary` | `text-primary-foreground` | 无 | `bg-primary/90` + `scale-[1.02]` | 立即购买、结算、支付、保存 |
| **Secondary** | `bg-card` | `text-foreground` | `border-border` | `bg-accent` + `border-primary` | 加入购物车、次要操作 |
| **Outline** | `transparent` | `text-foreground` | `border-border` | `bg-accent` | 取消、返回 |
| **Ghost** | `transparent` | `text-muted-foreground` | 无 | `bg-accent` + `text-foreground` | 文字按钮、链接 |
| **Danger** | `bg-destructive` | `text-white` | 无 | `bg-destructive/90` | 删除、取消订单 |
| **Disabled** | `bg-muted` | `text-muted-foreground` | 无 | `cursor-not-allowed` | 禁用状态 |

**尺寸规范**:
- 大按钮（CTA）: `h-12 px-8 text-base`（立即购买、去结算）
- 默认按钮: `h-10 px-4 py-2`（常规操作）
- 小按钮（图标/紧凑）: `h-8 px-3 text-sm`（编辑、删除、标签）
- 图标按钮: `h-10 w-10`（收藏、关闭）

### Form Elements

**输入框**:
- 背景: `bg-card`
- 边框: `border-border` → Focus: `border-primary ring-2 ring-primary/20`
- 圆角: `rounded-lg` (8px)
- 高度: `h-10` / `h-12`（大尺寸用于搜索框）
- Placeholder: `text-muted-foreground`

**搜索框（特殊）**:
- 圆角: `rounded-full`（与常规输入区分）
- 背景: `bg-muted` 或 `bg-card`
- 左侧搜索图标: `text-muted-foreground`
- 高度: `h-10` / `h-12`

**选择器（SKU选择）**:
- 选项卡片: `border rounded-lg px-4 py-2 cursor-pointer`
- 选中态: `border-primary bg-accent text-primary`
- 禁用态: `bg-muted text-muted-foreground cursor-not-allowed`（库存不足）

**复选框/单选框**:
- 选中: `bg-primary border-primary`
- 尺寸: `w-5 h-5 rounded`（与整体圆角风格一致，但 slightly smaller）

### Cards

**商品卡片**:
- 背景: `bg-card`
- 圆角: `rounded-lg`
- 阴影: `shadow-sm` → Hover: `shadow-md`
- 内边距: 图片区无内边距，信息区 `p-3` 或 `p-4`
- 结构: 图片（aspect-square, object-cover, rounded-t-lg）+ 标题（单行截断）+ 价格区

**订单卡片**:
- 背景: `bg-card`
- 圆角: `rounded-lg`
- 边框: `border`（强调边界）
- 内边距: `p-4` 或 `p-5`
- 头部: 订单号 + 状态标签（右侧）
- 内容: 商品缩略图列表（横向滚动或网格）+ 金额
- 底部: 操作按钮组（右对齐）

**信息卡片（地址选择）**:
- 选中态: `border-primary bg-accent/30`
- 默认态: `border-border bg-card`
- 圆角: `rounded-lg`
- 点击区域: 整卡可点击

### Navigation

**Topbar 导航项**:
- 默认: `text-muted-foreground`
- Hover: `text-foreground`
- 激活: `text-primary font-medium`

**Sidebar 导航项**:
- 布局: `flex items-center gap-3 px-4 py-3 rounded-lg`
- 默认: `text-sidebar-foreground hover:bg-sidebar-accent`
- 激活: `bg-sidebar-primary text-sidebar-primary-foreground font-medium`

### Tags & Badges

**状态标签**:
- 待付款: `bg-warning/10 text-warning border-warning/20`
- 待发货: `bg-primary/10 text-primary border-primary/20`
- 待收货: `bg-info/10 text-info border-info/20`
- 已完成: `bg-success/10 text-success border-success/20`
- 已取消: `bg-muted text-muted-foreground`

**商品标签（新品/热卖）**:
- 背景: `bg-primary` 或 `bg-destructive`（根据 urgency）
- 文字: `text-white text-xs font-medium`
- 圆角: `rounded-sm` 或 `rounded-full`
- 位置: 商品图片左上角（absolute）

**数量角标（购物车）**:
- 背景: `bg-primary`
- 文字: `text-white text-xs font-bold`
- 圆角: `rounded-full`
- 尺寸: `min-w-[20px] h-5 px-1.5`

### Skeleton 骨架屏

- 背景: `bg-muted`
- 动画: `animate-pulse`
- 圆角: 与对应组件一致（卡片 rounded-lg，文字 rounded）
- 用于: 商品列表加载、订单列表加载、图片占位

## 8. Flexibility Note (灵活性说明)

> **一致性优先原则**：前台与后台共享相同的设计语言（圆角、色彩、字体），仅在布局密度和导航方式上区分。所有页面必须使用相同的核心参数（主色 #FF4400、圆角 8px、字体 Inter）。
>
> **允许的微调范围**（code agent 可自行判断）：
> - 响应式断点适配（移动端商品网格 2列 → 桌面端 4-5列）
> - 后台表格页可根据内容需要调整列宽，但不改变整体 max-w 约束
> - 卡片内边距可根据内容多少在 `p-4` ~ `p-6` 间微调
> - 特定页面（如首页）可根据营销需要调整 Banner 高度（`h-64` ~ `h-96`）
>
> **禁止的随意变更**：
> - ❌ 前台与后台使用不同的主色调
> - ❌ 部分页面使用 4px 圆角、部分使用 16px 圆角（必须统一 8px）
> - ❌ 引入第三种字体（保持 Inter + 系统回退）
> - ❌ 深色主题（需求明确要求浅色简洁风格）

## 9. Signature & Constraints (设计签名与禁区)

### DO (视觉签名)

1. **8px 圆角统一体系**: 从按钮到卡片到输入框，全部使用 `rounded-lg`（8px），创造一致柔和的触感
   ```css
   --radius: 0.5rem; /* 8px */
   ```

2. **高对比红色 CTA**: 主按钮使用 `#FF4400`（hsl(15 100% 50%)）配白色文字，确保行动召唤清晰可见
   ```css
   bg-[hsl(15_100%_50%)] text-white font-semibold
   ```

3. **充足的卡片留白**: 商品卡片信息区使用 `p-4`（16px），确保文字不贴边，提升阅读舒适度

4. **清晰的价格层级**: 现价 `text-xl font-bold text-primary`，原价 `text-sm text-muted-foreground line-through`，对比强烈

5. **毛玻璃导航**: Topbar 使用 `backdrop-blur` + 半透明背景，滚动时保持内容可见性同时提供导航锚点

### DON'T (禁止做法)

- ❌ 使用 Tailwind 预设色板（如 `bg-blue-500`、`text-green-600`）替代设计系统颜色（必须使用 HSL 定义的颜色）
- ❌ 商品卡片使用 `shadow-lg` 或 `shadow-xl`（默认保持 `shadow-sm`，仅 hover 时 `shadow-md`）
- ❌ 在商品标题上使用 `line-clamp-2` 以上（保持单行或双行，避免视觉混乱）
- ❌ 后台 Sidebar 使用深色背景（保持与前台一致的浅色清爽风格）
- ❌ 移动端使用 `hover` 状态（移动端无 hover，需使用 active 态或无需反馈）
- ❌ 使用 `w-full` 让内容在大屏幕上无限延伸（必须使用 `max-w-6xl` 或 `max-w-[1400px]` 约束）