# Popover 弹窗

点击元素进行隐藏内容的弹窗展示。

## 代码用例
<!--code-->
## API
### Props

属性|类型|描述|默认值
---|---|---|---
name|String|设置触发指令关联的`name`值|-
value|Boolean|弹窗的显示状态，可使用`v-model`双向绑定|false
placement|String|设置弹窗显示的位置|`bottom`
eventsEnabled|Boolean|启用一些监听事件（`resize`、`scroll`），用于位置更新|`true`
offset|Number|弹窗的位置偏移量|`0`
arrow|Boolean|是否显示箭头|`true`
width|Number/String|设置弹窗面板宽度|预设最小宽度`150px`
zIndex|Number|设置弹窗层级，一般不需要设置，组件内部实现了`zIndex`递增|-
fullHeight|Boolean|是否自动填充高度到屏幕边界|-
transitionName|String|设置弹窗显示的过渡名称（`transition`）|`fade-in`
mountPoint|Boolean/String|弹窗挂载点。<br>`false`: 组件当前位置；`true`:`document.body`；`String`:传入一个 DOM 选择器。|`true`
reset|Boolean|弹窗关闭时是否保留状态（为`true`时将使用`v-if`控制显示状态）|`false`
contentClass|String|弹窗内容面板的自定义样式类|-
hoverDelay|Number|`hover`触发时的延迟显示设置，毫秒|300

### Events

事件|描述|参数
---|---|---
change|显示状态变化时触发|`visible`：是否可见
show|弹窗显示时触发|-
hide|弹窗关闭时触发|-
createPopover|弹窗创建时触发|`dataObject` [参考](https://popper.js.org/popper-documentation.html#dataObject)
updatePopover|弹窗更新时触发|`dataObject`

### v-popover

参数|描述|示例
---|---|---
arg|指令参数与要关联的`popover`组件的`name`属性值一致。|`v-popover:hello`
modifiers.[trigger]|表示触发的事件，支持：`click`、`hover`、`focus`。省略该项时为`click`。|`v-popover:hello.hover`
modifiers.name|动态`name`，比如在一个循环中，你无法拼接一个指令`arg`值，你可以使用这种方式。|`v-popover.name="popName"`
modifiers.only|定义单例的弹窗|`v-popover.only="settings"`
value|单例弹窗时传入单例弹窗及弹窗内显示内容组件的参数和事件等；或者动态`name`传入一个字符串|-

## 单例弹窗

### 全局配置

仅用一个唯一的`props`用于公共的`popover`设置，可设置项对应`popover:props`。

```html
<ui-popover-holder :settings="yourSettings"></ui-popover-holder>
```

### 指令值数据结构参考

```js
{
    component, // 弹窗里的内容组件

    // 内容组件的 props 参数
    /* 略... */

    events: {}, // 内容组件的事件监听

    popover: { // popover 参数传递，优先级高于全局配置

        // popover: props
        /* 略... */

        events: {} // popover: events
    }
}
```

### 内部关闭弹窗

单例弹窗内部实现了一个关闭弹窗的事件监听，内容组件需要关闭弹窗时，派发一个关闭事件即可。

如下，也可参考 **单例** 中的`Click3`相关代码：

```js
export default {
    methods: {
        onHide() {
            this.$emit('close:popover')
        }
    }
}
```

### 也可以更Open

可以在组件内使用`this`调用单例弹窗实例，去完成一些不可思议的操作。

```js
export default {
    watch: {
        $route() {
            // 路由改变时关闭弹窗
            this.$popover.visible = false
        }
    }
}
```

更多示例需要你去阅读源码发现。
