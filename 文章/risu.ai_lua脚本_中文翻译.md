
# Risu.ai Lua 脚本指南

在触发器脚本的 Lua 模式下，您可以编写 Lua 脚本来处理触发器事件。当触发器事件被触发时，该脚本将会被执行。

## 回调函数

在 Lua 脚本中，您可以使用三个常规的回调函数：

- `onStart(triggerId)`: 当聊天消息被发送时，将调用此函数。
- `onOutput(triggerId)`: 当接收到 AI 回应时，将调用此函数。
- `onInput(triggerId)`: 当接收到用户输入时，将调用此函数。

对于按钮事件（例如 `{{button::显示文本::触发器名称}}`），它会调用与 `触发器名称` 同名的函数。

您还可以使用 `listenEdit(type, callback)` 来监听编辑事件。`type` 的类型可以是 `editRequest`、`editDisplay`、`editInput` 和 `editOutput`。其回调函数格式为 `callback(triggerId, data)`。

---

## 示例

**示例 1：监听用户输入**

```lua
function onInput(triggerId)
    alertNormal(triggerId, "已接收到输入！")
end
```
*上述脚本会在接收到用户输入时，弹出一个内容为“已接收到输入！”的普通提示框。*

---

**示例 2：编辑显示内容**

```lua
listenEdit("editDisplay", function(triggerId, data)
    local prefix = getState(triggerId, "prefix") or ""
    return prefix .. data
end)
```
*上述脚本会将状态变量中存储的 `prefix`（前缀）附加到显示消息的前面。*

---

**示例 3：监听按钮点击**

```lua
function onButton(triggerId)
    alertNormal(triggerId, "按钮被点击了！")
end
```
*当 `{{button::显示文本::onButton}}` 这个按钮被点击时，上述脚本会弹出一个内容为“按钮被点击了！”的普通提示框。*

---

## 内置函数

您可以在 Lua 脚本中使用以下函数。出于安全原因，除了 `log` 之外，每个函数都必须以 `triggerId` 作为第一个参数来调用。您可以从回调函数中获取 `triggerId`。

**注意**：除了 `setChatVar`, `getChatVar`, `setState`, `getState`, `log` 之外的所有函数，在 `editDisplay` 事件中都无法工作。

### 日志
`log(message)`
- 将消息记录到控制台。与 `print` 不同，它以 JSON 格式输出，使得数组和对象能在开发者工具控制台中正确显示。

### 聊天变量
`setChatVar(triggerId, key, value)`
- 用指定的 `key` 和 `value` 设置一个聊天变量。`value` 必须是字符串。

`getChatVar(triggerId, key)`
- 获取指定 `key` 的聊天变量的值。

### 状态变量
`setState(triggerId, key, value)`
- 用指定的 `key` 和 `value` 设置一个状态变量。`value` 必须是可序列化为 JSON 的值（如字符串、数字、布尔值、数组、对象）。

`getState(triggerId, key)`
- 获取指定 `key` 的状态变量的值。

### 聊天控制
`stopChat(triggerId)`
- 停止聊天。这只在某些触发器事件中有效。

`cutChat(triggerId, start, end)`
- 从 `start` 到 `end` 裁剪聊天记录。`start` 和 `end` 都是从 0 开始的索引。如果只想删除单条消息，请使用 `removeChat(triggerId, index)`。

`removeChat(triggerId, index)`
- 移除指定 `index` 处的聊天消息。`index` 从 0 开始。

`addChat(triggerId, role, message)`
- 添加一条带有指定 `role` 和 `message` 的聊天消息。`role` 可以是 `user`（用户）和 `char`（角色）。

`insertChat(triggerId, index, role, message)`
- 在指定 `index` 处插入一条带有 `role` 和 `message` 的聊天消息。`index` 从 0 开始。`role` 可以是 `user` 和 `char`。

`getChatLength(triggerId)`
- 获取聊天记录的长度。

`setChat(triggerId, index, message)`
- 设置指定 `index` 处的聊天消息。`index` 从 0 开始。

`setChatRole(triggerId, index, role)`
- 设置指定 `index` 处的聊天角色。`index` 从 0 开始。`role` 可以是 `user` 和 `char`。

`getFullChat(triggerId)`
- 获取完整的聊天记录，格式如下：
    ```lua
    {
        {role = "user", data = "用户消息"},
        {role = "char", data = "角色消息"},
        ...
    }
    ```

`setFullChat(triggerId, chat)`
- 设置完整的聊天记录，格式与 `getFullChat` 相同。

### 弹窗提示
`alertError(triggerId, message)`
- 显示一个带有 `message` 的错误提示框。

`alertNormal(triggerId, message)`
- 显示一个带有 `message` 的普通提示框。

`alertInput(triggerId, message)`
- 显示一个带有 `message` 的输入提示框，并返回输入的值。

### 角色设定
`getName(triggerId)`
- 返回当前角色的名称。

`setName(triggerId, name)`
- 设置当前角色的名称。

`getDescription(triggerId)`
- 返回当前角色的描述。

`setDescription(triggerId, description)`
- 设置当前角色的描述。

`getCharacterFirstMessage(triggerId)`
- 返回当前角色的第一条消息。

`setCharacterFirstMessage(triggerId, message)`
- 设置当前角色的第一条消息。

`getBackgroundEmbedding(triggerId)`
- 返回当前角色的背景嵌入。

`setBackgroundEmbedding(triggerId, embedding)`
- 设置当前角色的背景嵌入。

### 高级功能 (需要底层访问权限)

`similarity(triggerId, source, value)`
- 在 `source` 和 `value` 之间执行相似度排序。`source` 必须是一个字符串数组，`value` 必须是一个字符串。返回一个按相似度排序的索引数组。
- 这是一个**异步函数**。使用 `similarity(triggerId, source, value):await()` 来等待结果。

`generateImage(triggerId, prompt, negative)`
- 根据 `prompt`（正向提示）和 `negative`（反向提示）生成一张图片。返回图片的资源 CBS (例如 `{{asset::assetId}}`)。
- 这是一个**异步函数**。使用 `generateImage(triggerId, prompt, negative):await()` 来等待结果。

`LLM(triggerId, data)`
- 使用 `data` 执行一次 LLM（大语言模型）请求。返回响应结果。
- `data` 必须是消息格式，结构如下：
    ```lua
    {
        -- 消息角色，可以是 "system", "assistant", "user"
        {
            role = "system",
            data = "系统消息"
        },
        {
            role = "assistant",
            data = "角色消息"
        },
        {
            role = "user",
            data = "用户消息"
        }
        ...
    }
    ```
- 响应结果的格式为：
    ```lua
    {
        success = true,
        message = "响应消息",
    }
    ```
- 这是一个**异步函数**。使用 `LLM(triggerId, data):await()` 来等待结果。

`simpleLLM(triggerId, message)`
- 使用 `message` 执行一次简单的 LLM 请求。返回响应结果。`message` 和响应结果都是字符串格式。
- 这是一个**异步函数**。使用 `simpleLLM(triggerId, message):await()` 来等待结果。

---

## 小贴士

- 如果您不知道如何编写 Lua 脚本，可以参考 Lua官方手册 或向 RisuAI 聊天乐园等 AI 聊天机器人寻求帮助。
- 由于应用程序只为 Lua 脚本提供了一个小型的图形用户界面（GUI），我们建议使用像 VSCode 这样的外部编辑器来编写脚本，然后复制粘贴到应用程序中。
- 我们推荐使用 `log` 而不是 `alert` 或 `print` 进行调试，因为它不会干扰其他功能，并且能以更易读的格式显示数据。
- 我们推荐使用 `setState` 和 `getState` 来存储数据，而不是 `setChatVar` 和 `getChatVar`，因为它支持更多的数据类型。
- 每次触发器事件被触发时，Lua 脚本都会从头开始执行。因此，如果您想保留某些数据，必须使用聊天变量或状态变量。
- Lua 环境中包含了 `json.lua`，所以您可以使用 `json.encode` 和 `json.decode` 来序列化和反序列化 JSON 数据。
- Lua 脚本在沙盒环境中执行，因此您无法访问文件系统或网络。
- 除了内置的 `json.lua`，您不能使用外部库。
- 如果 Lua 脚本中发生错误，应用程序的运行将如同脚本未被执行一样，错误消息会显示在控制台中。
- 此处的 Lua 版本基于 Lua 5.4。