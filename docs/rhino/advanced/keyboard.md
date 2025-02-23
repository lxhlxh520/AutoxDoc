# keyboard 键盘输入

该模块通过输入法向界面中输入和编辑文本

要使用该模块必须在系统设置中启用并激活 autox 输入法

通过检查`keyboard.open`返回`true`确保输入法已经打开才能够进行输入操作

## keyboard.open

- return \{ boolean } 输入法界面已打开为`true`,否则为`false`

## keyboard.available

- return \{ boolean } 输入法处于可输入状态将为`true`,否则为`false`

在某些应用中即使没有打开输入法也能够输入内容，但这些内容可能会被应用忽略

## keyboard.addInputQueue(text)

- text \{ string } 待输入文本
- return \{ boolean } 返回操作是否成功

添加一段文本到待输入列队，如果输入法已打开则立即输入，否则将在下次打开输入法时输入
输入列队是全局的，与其他脚本共用

## keyboard.clearInputQueue()

清空输入列队

## keyboard.inputText(text)

- text \{ string } 待输入文本
- return \{ boolean } 返回操作是否成功

立刻输入一段文本

## keyboard.downKeyCode(code)

- code \{ number } 按键代码
- return \{ boolean } 返回操作是否成功

模拟按下一个按键，按键码参考[对照表](../base/keys.md#keycodecode)

## keyboard.upKeyCode(code)

- code \{ number } 按键代码
- return \{ boolean } 返回操作是否成功

模拟弹起一个按键，按键码参考[对照表](../base/keys.md#keycodecode)

## keyboard.getInputText()

- return \{ string }

获取输入框中的文本，该函数如果`keyboard.available`为`false`则会抛出错误

## keyboard.setSelection(start [, end])

- start \{ number } 开始位置
- end \{ number } 结束位置，默认值为 start
- return \{ boolean } 返回操作是否成功

选择一段内容,如果省略 end 参数将会调整光标位置

## keyboard.clearInput()

- return \{ boolean } 返回操作是否成功

清空输入框中的内容

## keyboard.deleteSurroundingText(beforeLength [, afterLength])

- beforeLength \{ number } 光标左边长度
- afterLength \{ number } 光标右边长度，默认值为 0
- return \{ boolean } 返回操作是否成功

删除光标左边和右边一定长度内容，更具安卓开发者文档描述，传入越界参数可能会造成不良影响

## keyboard.getSelectedText()

- return \{ string }

获取光标选择中的文本，可能返回 null
