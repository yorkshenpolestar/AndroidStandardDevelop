# Polestar Android 开发规范

## 摘要

* [1 前言](#1-前言)
* [2 AS 规范](#2-as-规范)
todo


### 1 前言

为了有利于项目维护、增强代码可读性、提升 Code Review 效率以及规范团队安卓开发，故提出以下安卓开发规范，如有更好建议，欢迎完善此文档。后续可能会根据该规范出一个 CheckStyle 插件来检查是否规范，当然也支持在 CI 上运行。


### 2 AS 规范

工欲善其事，必先利其器。

1. 尽量使用最新的稳定版的 IDE 进行开发；
2. 编码格式统一为 **UTF-8**；
3. 编辑完 .java、.kt、.xml 等文件后一定要 **格式化，格式化，格式化**；
4. 删除多余的 import，减少警告出现，可利用 AS 的 Optimize Imports（Settings -> Keymap -> Optimize Imports）快捷键；


### 3 资源文件命名规范

代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。正确的英文拼写和语法可以让阅读者易于理解，避免歧义，资源文件命名为全部小写，需带模块前缀，采用下划线命名法。

我们是模块化化开发，另有通用组件库[ui-component-android](https://github.com/polestarchina/ui-component-android), 通用的项目组建请放在组件库中。另外可在公共模块lib_base中专门存放资源文件，然后让每个module都依赖lib_base模块。

> 注意：即使纯拼音命名方式也要避免采用。但 `alibaba`、`taobao`、`youku`、`hangzhou` 等国际通用的名称，可视同英文。


#### 3.1 `anim` 资源文件

命名规则：`模块名_逻辑名称_[方向｜序号]`。

说明：`逻辑名称` 可由多个单词加下划线组成。

例如：`base_refresh_progress.xml`、`home_cart_add.xml`、`home_cart_remove.xml`。

如果是普通的补间动画或者属性动画，可采用：`动画类型_方向` 的命名方式。

例如：

| 名称                  | 说明      |
| ------------------- | ------- |
| `fade_in`           | 淡入      |
| `fade_out`          | 淡出      |
| `push_down_in`      | 从下方推入   |
| `push_down_out`     | 从下方推出   |
| `push_left`         | 推向左方    |
| `slide_in_from_top` | 从头部滑动进入 |
| `zoom_enter`        | 变形进入    |
| `slide_in`          | 滑动进入    |
| `shrink_to_middle`  | 中间缩小    |

#### 3.2 `drawable` 资源文件

命名规则：`模块名_业务功能描述_控件描述_控件状态限定词`。

说明：`逻辑名称` 可由多个单词加下划线组成。

例如：`refresh_progress.xml`、`market_cart_add.xml`、`market_cart_remove.xml`、`module_login_btn_pressed.xml`、`module_tabs_icon_home_normal.xml`。

#### 3.3 布局资源文件（layout/）

命名规则：以 `模块名_类型_` 开头

例如：

| 名称                          | 说明                          |
| --------------------------- | --------------------------- |
| `module_activity_main.xml`         |                 |
| `module_include_header.xml`        | include 的layout以此开头        |
| `module_fragment_music.xml`        |                |
| `module_fragment_music_player.xml` |      |
| `module_dialog_loading.xml`        | 加载对话框 `类型_逻辑名称`             |
| `module_recycle_item_settings.xml` | RecyclerView 的 item layout 以此开头 |

#### [推荐]3.4 color 资源


`<color>` 的 `name` 命名使用下划线命名法，在你的 `colors.xml` 文件中应该只是映射颜色的名称一个 ARGB 值，而没有其它的。不要使用它为不同的按钮来定义 ARGB 值。

例如，不要像下面这样做：

```xml
  <resources>
      <color name="button_foreground">#FFFFFF</color>
      <color name="button_background">#2A91BD</color>
      <color name="comment_background_inactive">#5F5F5F</color>
      <color name="comment_background_active">#939393</color>
      <color name="comment_foreground">#FFFFFF</color>
      <color name="comment_foreground_important">#FF9D2F</color>
      ...
      <color name="comment_shadow">#323232</color>
```

使用这种格式，会非常容易重复定义 ARGB 值，而且如果应用要改变基色的话会非常困难。同时，这些定义是跟一些环境关联起来的，如 `button` 或者 `comment`，应该放到一个按钮风格中，而不是在 `colors.xml` 文件中。

相反，应该这样做：

```xml
  <resources>

      <!-- grayscale -->
      <color name="white"     >#FFFFFF</color>
      <color name="gray_light">#DBDBDB</color>
      <color name="gray"      >#939393</color>
      <color name="gray_dark" >#5F5F5F</color>
      <color name="black"     >#323232</color>

      <!-- basic colors -->
      <color name="green">#27D34D</color>
      <color name="blue">#2A91BD</color>
      <color name="orange">#FF9D2F</color>
      <color name="red">#FF432F</color>

  </resources>
```

向应用设计者那里要这个调色板，名称不需要跟 `"green"`、`"blue"` 等等相同。`"brand_primary"`、`"brand_secondary"`、`"brand_negative"` 这样的名字也是完全可以接受的。像这样规范的颜色很容易修改或重构，会使应用一共使用了多少种不同的颜色变得非常清晰。通常一个具有审美价值的 UI 来说，减少使用颜色的种类是非常重要的。

> 注意：如果某些颜色和主题有关，那就单独写一个 `colors_theme.xml`。


#### 3.5 strings.xml

`string` 资源文件或者文本用到字符需要全部写入 `lib_base` 下的 `value/strings.xml` 文件中，`<string>` 的 `name` 命名使用下划线命名法，采用以下规则：`{模块名_}逻辑名称`，这样方便同一个界面的所有 `string` 都放到一起，方便查找。

说明：`{}` 中的内容为可选。


| 名称                  | 说明      |
| ------------------- | ------- |
| `main_menu_about`   | 主菜单按键文字 |
| `friend_title`      | 好友模块标题栏 |
| `friend_dialog_del` | 好友删除提示  |
| `login_check_email` | 登录验证    |
| `dialog_title`      | 弹出框标题   |
| `button_ok`         | 确认键     |
| `loading`           | 加载文字    |
| `modulename_home_page_notice_desc`           | 提示文字    |
| `modulename_login_tips`           | 提示文字    |



#### 3.6 dimens.xml

像对待 `colors.xml` 一样对待 `dimens.xml` 文件，与定义颜色调色板一样，你同时也应该定义一个空隙间隔和字体大小的“调色板”。 一个好的例子，如下所示：

```xml
<resources>

    <!-- font sizes -->
    <dimen name="font_22">22sp</dimen>
    <dimen name="font_18">18sp</dimen>
    <dimen name="font_15">15sp</dimen>
    <dimen name="font_12">12sp</dimen>

    <!-- typical spacing between two views -->
    <dimen name="spacing_40">40dp</dimen>
    <dimen name="spacing_24">24dp</dimen>
    <dimen name="spacing_14">14dp</dimen>
    <dimen name="spacing_10">10dp</dimen>
    <dimen name="spacing_4">4dp</dimen>

    <!-- typical sizes of views -->
    <dimen name="button_height_60">60dp</dimen>
    <dimen name="button_height_40">40dp</dimen>
    <dimen name="button_height_32">32dp</dimen>

</resources>
```

布局时在写 `margins` 和 `paddings` 时，你应该使用 `spacing_xx` 尺寸格式来布局，而不是像对待 `string` 字符串一样直接写值，像这样规范的尺寸很容易修改或重构，会使应用所有用到的尺寸一目了然。 这样写会非常有感觉，会使组织和改变风格或布局非常容易。


#### 3.7 styles.xml

`<style>` 的 `name` 命名使用大驼峰命名法，几乎每个项目都需要适当的使用 `styles.xml` 文件，因为对于一个视图来说，有一个重复的外观是很常见的，将所有的外观细节属性（`colors`、`padding`、`font`）放在 `styles.xml` 文件中。 在应用中对于大多数文本内容，最起码你应该有一个通用的 `styles.xml` 文件，例如：

```
<style name="ContentText">
    <item name="android:textSize">@dimen/font_normal</item>
    <item name="android:textColor">@color/basic_black</item>
</style>
```

应用到 `TextView` 中：

```
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/price"
    style="@style/ContentText"
    />
```

或许你需要为按钮控件做同样的事情，不要停止在那里，将一组相关的和重复 `android:xxxx` 的属性放到一个通用的 `<style>` 中。

### 3.8 id 命名

Id 资源原则上以驼峰法命名， View 组件的资源 id 需要以 View 的缩写作为前缀。常用缩写表如下：

![image](https://user-images.githubusercontent.com/18043540/160287673-73e97112-8597-4db5-92b9-3e8e1a90291b.png)

其它控件的缩写推荐使用小写字母并用下划线进行分割，例如

DatePicker 对应的缩写为 date_picker

### 3.9 图片资源文件（drawable/ 和 mipmap/）

`res/drawable/` 目录下放的是位图文件（.png、.9.png、.jpg、.gif）或编译为可绘制对象资源子类型的 XML 文件，而 `res/mipmap/` 目录下放的是不同密度的启动图标，所以 `res/mipmap/` 只用于存放启动图标，其余图片资源文件都应该放到 `res/drawable/` 目录下。

> [Put app icons in mipmap directories](https://developer.android.com/training/multiscreen/screendensities#mipmap), [mipmap vs drawable](https://wanandroid.com/wenda/show/17666)

命名规则：`模块名_类型_逻辑名称`、`模块名_类型_颜色`。

说明：`类型` 可以是[可绘制对象资源类型](可绘制对象资源类型)，也可以是控件类型（具体见附录[UI 控件缩写表](#ui-控件缩写表)）；最后可加后缀 `_small` 表示小图，`_big` 表示大图。

例如：

| 名称                        | 说明                       |
| ------------------------- | ------------------------ |
| `main_btn_about.png`      | 主页关于按键 `模块名_类型_逻辑名称`     |
| `base_btn_back.png`            | 返回按键 `类型_逻辑名称`           |
| `maket_divider_white.png` | 商城白色分割线 `模块名_类型_颜色`      |
| `base_ic_edit.png`             | 编辑图标 `类型_逻辑名称`           |
| `main_bg.png`             | 主页背景 `类型_逻辑名称`           |
| `base_btn_red.png`             | 红色按键 `类型_颜色`             |
| `base_btn_red_big.png`         | 红色大按键 `类型_颜色`            |
| `base_ic_head_small.png`       | 小头像图标 `类型_逻辑名称`          |
| `base_bg_input.png`            | 输入框背景 `类型_逻辑名称`          |
| `base_divider_white.png`       | 白色分割线 `类型_颜色`            |
| `bg_main_head.png`        | 主页头部背景 `类型_模块名_逻辑名称`     |
| `base_ic_def_search.png`     | 默认搜索 icon |
| `base_ic_more_help.png`        | 更多帮助图标        |
| `divider_list_line.png`   | 列表分割线 `类型_逻辑名称`          |
| `base_music_ring.xml`    | 音乐界面环形形状 `类型_逻辑名称`   |

如果有多种形态，如按钮选择器：`selector_btn_xx.xml`，采用如下命名：

| 名称                      | 说明                            |
| ----------------------- | ----------------------------- |
| `selector_btn_xx`            | 作用在 `btn_xx` 上的 `selector`    |
| `btn_xx_normal`         | 默认状态效果                        |
| `btn_xx_pressed`        | `state_pressed` 点击效果          |
| `btn_xx_focused`        | `state_focused` 聚焦效果          |
| `btn_xx_disabled`       | `state_enabled` 不可用效果         |
| `btn_xx_checked`        | `state_checked` 选中效果          |
| `btn_xx_selected`       | `state_selected` 选中效果         |
| `btn_xx_hovered`        | `state_hovered` 悬停效果          |
| `btn_xx_checkable`      | `state_checkable` 可选效果        |
| `btn_xx_activated`      | `state_activated` 激活效果        |
| `btn_xx_window_focused` | `state_window_focused` 窗口聚焦效果 |

> 注意：使用 Android Studio 的插件 SelectorChapek 可以快速生成 selector，前提是命名要规范。


### 3.10 values 资源文件（values/）

`values/` 资源文件下的文件都以 `s` 结尾，如 `attrs.xml`、`colors.xml`、`dimens.xml`，起作用的不是文件名称，而是 `<resources>` 标签下的各种标签，比如 `<style>` 决定样式，`<color>` 决定颜色，所以，可以把一个大的 `xml` 文件分割成多个小的文件，比如可以有多个 `style` 文件，如 `styles.xml`、`styles_home.xml`、`styles_item_details.xml`、`styles_forms.xml`。


### 3.11 第三方库规范


希望 Team 能用时下较新的技术，对开源库的选取，一般都需要选择比较稳定的版本，作者在维护的项目，要考虑作者对 issue 的解决，以及开发者的知名度等各方面。选取之后，一定的封装是必要的。

已在项目中使用的有以下三方库：

* **[Retrofit][Retrofit]**
* **[RxAndroid][RxAndroid]**
* **[OkHttp][OkHttp]**
* **[Glide][Glide]**
* **[Gson][Gson]**
* **[EventBus][EventBus]**
* **[GreenDao][GreenDao]**
* **[Dagger2][Dagger2]**

## 4. 注释规范

为了减少他人阅读你代码的痛苦值，请在关键地方做好注释。

### 4.1 类注释

每个类完成后应该有作者姓名和联系方式的注释，对自己的代码负责。

```java
/**
 * <pre>
 *     author : York Shen
 *     e-mail : xxx@xx
 *     time   : 2022/03/28
 *     desc   : xxxx 描述
 *     version: 1.0
 * </pre>
 */
public class WelcomeActivity {
    ...
}
```

具体可以在 AS 中自己配制，进入 Settings -> Editor -> File and Code Templates -> Includes -> File Header，输入

```java
/**
 * <pre>
 *     author : ${USER}
 *     e-mail : xxx@xx
 *     time   : ${YEAR}/${MONTH}/${DAY}
 *     desc   :
 *     version: 1.0
 * </pre>
 */
```

这样便可在每次新建类的时候自动加上该头注释。


### 4.2 方法注释

在一些关键的方法的方法头都必须做方法头注释，在方法前一行输入 `/** + 回车` 或者设置 `Fix doc comment`（Settings -> Keymap -> Fix doc comment）快捷键，AS 便会帮你生成模板，我们只需要补全参数即可，如下所示。

```kotlin
/**
     * 当用户完成期数和首付选择后前端刷新贷款金额结果时上报
     * @param order_ID
     * @param configuration_ID
     * @param loan_provider 贷款机构
     * @param loan_periods 期数
     * @param loan_down_payment_rate 首付比率
     * @param loan_rate 费率
     * @param loan_amount 贷款金额
     * @param loan_monthly_payment 贷款月供
     * @param financial_product 标准产品
     *
     */
    fun financialCalculatorUsage(
        order_ID: String,
        configuration_ID: String,
        loan_provider: String,
        loan_periods: String,
        loan_down_payment_rate: String,
        loan_rate: String,
        loan_amount: String,
        loan_monthly_payment: String,
        financial_product: String
    ) {
        val pty = JSONObject()
        pty.put("order_ID", order_ID)
        pty.put("configuration_ID", configuration_ID)
        pty.put("loan_provider", loan_provider)
        pty.put("loan_periods", loan_periods)
        pty.put("loan_down_payment_rate", loan_down_payment_rate)
        pty.put("loan_rate", loan_rate)
        pty.put("loan_amount", loan_amount)
        pty.put("loan_monthly_payment", loan_monthly_payment)
        pty.put("financial_product", financial_product)
        SensorsDataAPI.sharedInstance().track("financialCalculatorUsage", pty)
    }

     */
```


### 4.3 块注释

块注释与其周围的代码在同一缩进级别。它们可以是 `/* ... */` 风格，也可以是 `// ...` 风格（**`//` 后最好带一个空格**）。对于多行的 `/* ... */` 注释，后续行必须从 `*` 开始， 并且与前一行的 `*` 对齐。以下示例注释都是 OK 的。

```java
/*
 * This is
 * okay.
 */

// And so
// is this.

/* Or you can
* even do this. */
```

注释不要封闭在由星号或其它字符绘制的框架里。

> Tip：在写多行注释时，如果你希望在必要时能重新换行（即注释像段落风格一样），那么使用 `/* ... */`。


#### 4.4 其他一些注释

AS 已帮你集成了一些注释模板，我们只需要直接使用即可，在代码中输入 `todo`、`fixme` 等这些注释模板，回车后便会出现如下注释。

```java
// TODO: 22/3/28 需要实现，但目前还未实现的功能的说明
// FIXME: 22/3/28 需要修正，甚至代码是错误的，不能工作，需要修复的说明
```

## 附录

### UI 控件缩写表

| 名称             | 缩写   |
| -------------- | ---- |
| Button         | btn  |
| CheckBox       | cb   |
| EditText       | et   |
| FrameLayout    | fl   |
| GridView       | gv   |
| ImageButton    | ib   |
| ImageView      | iv   |
| LinearLayout   | ll   |
| ListView       | lv   |
| ProgressBar    | pb   |
| RadioButtion   | rb   |
| RecyclerView   | rv   |
| RelativeLayout | rl   |
| ScrollView     | sv   |
| SeekBar        | sb   |
| Spinner        | spn  |
| TextView       | tv   |
| ToggleButton   | tb   |
| VideoView      | vv   |
| WebView        | wv   |

### 常见的英文单词缩写表

| 名称                   | 缩写                                       |
| -------------------- | ---------------------------------------- |
| average              | avg                                      |
| background           | bg（主要用于布局和子布局的背景）                        |
| buffer               | buf                                      |
| control              | ctrl                                     |
| current              | cur                                      |
| default              | def                                      |
| delete               | del                                      |
| document             | doc                                      |
| error                | err                                      |
| escape               | esc                                      |
| icon                 | ic（主要用在 App 的图标）                         |
| increment            | inc                                      |
| information          | info                                     |
| initial              | init                                     |
| image                | img                                      |
| Internationalization | I18N                                     |
| length               | len                                      |
| library              | lib                                      |
| message              | msg                                      |
| password             | pwd                                      |
| position             | pos                                      |
| previous             | pre                                      |
| selector             | sel（主要用于某一 view 多种状态，不仅包括 ListView 中的 selector，还包括按钮的 selector） |
| server               | srv                                      |
| string               | str                                      |
| temporary            | tmp                                      |
| window               | win                                      |

程序中使用单词缩写原则：不要用缩写，除非该缩写是约定俗成的。
