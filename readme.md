# 90 年代光标特效

_「懂点代码」曾经风靡一时，我想把其中一些带回来。_

这个仓库收录了那些曾激发全球创意、让人想学点代码的老式特效。经过现代化改造，它们更高效，同样烦人（也更有趣），和当年一样。[在这里玩玩看](https://tholman.com/cursor-effects)。

当前包含的特效：

- Rainbow Cursor（彩虹光标）
- Emoji Rain（表情雨）
- Elastic Emoji（弹性表情）
- Ghost Following（幽灵跟随）
- Trailing Cursor（拖尾光标）
- Text Flag Cursor（文字旗帜光标）
- Following Dot（跟随圆点）
- Bubbles Particles（气泡粒子）
- Snowflake Particles（雪花粒子）
- Fairy Dust（仙女尘）
- Clock Cursor（时钟光标）
- Character Cursor（字符光标）

# 本地搭建 / 开发

1. 先安装依赖（只有 rollup，用于代码编译）：`npm install`
2. 运行 `npm run watch`。这会把 `src` 编译到 `index.html` 所引用的 `dist` 文件夹，并在文件变更时自动更新。然后在任意浏览器中打开 `index.html` 即可。

# 使用方法

在网页中加入以下 script 标签（若要通过 npm 使用本包，见下一节）。脚本加载完成后，即可在页面上添加特效。

```html
<script src="https://unpkg.com/cursor-effects@latest/dist/browser.js"></script>
```

较新的浏览器也可以使用 `type="module"` 脚本配合 import 语句。若使用 ESM 模块，可按需导入具体光标效果，而不必使用 `cursoreffects.x` 这种写法。

```html
<script type="module">
  import { fairyDustCursor } from "https://unpkg.com/cursor-effects@latest/dist/esm.js";

  new fairyDustCursor();
</script>
```

然后在 JavaScript 中创建对应类型的实例。脚本会自动创建所需 canvas，一般无需其他配置。

```js
window.addEventListener("load", (event) => {
  new cursoreffects.ghostCursor();
});
```

也可以指定特定元素，让 canvas 出现在该元素内部，例如：

```js
const targetElement = document.querySelector("#ghost");
new cursoreffects.ghostCursor({ element: targetElement });
```

若要移除特效，可调用 `destroy` 方法。

```js
// 创建
let cursorEffect = new ghostCursor();

// 销毁
cursorEffect.destroy();
```

### 或通过 NPM 使用

```sh
npm install cursor-effects
```

```js
import { emojiCursor } from "cursor-effects";
new emojiCursor({ emoji: ["🔥", "🐬", "🦆"] });
```

## 自定义选项

部分特效支持自定义配置（如需更多选项，欢迎提 issue 或 PR）。

### ghostCursor

可在 `ghostCursor` 中更换光标图片。

```js
  new cursoreffects.ghostCursor(
  {image:"https://www.cursor.cc/cursor/820/63/cursor.png"}
  );
});
```

### rainbowCursor

可在 `rainbowCursor` 中调整颜色、大小和长度。

```js
new cursoreffects.rainbowCursor({
  length: 3,
  colors: ["red", "blue"],
  size: 4,
});
```

### springyEmojiCursor

可在 `springyEmojiCursor` 中通过 `emoji` 选项（单个 emoji 字符串）更换表情。

```js
new cursoreffects.springyEmojiCursor({ emoji: "🤷‍♂️" });
```

### bubbleCursor

可在 `bubbleCursor` 中通过 `fillColor` 和 `strokeColor` 选项调整气泡填充色与描边/边框颜色。

```js
new cursoreffects.bubbleCursor({
  fillColor: "#f771b4",
  strokeColor: "#e6f1f7",
});
```

### fairyDustCursor

可通过 `fairySymbol` 选项更换 emoji，通过 `colors` 选项（数组）定义拖尾颜色，来自定义 `fairyDustCursor`。

```js
new cursoreffects.fairyDustCursor({
  colors: ["#ff0000", "#00ff00", "#0000ff"],
  fairySymbol: "★",
});
```

### emojiCursor

可通过 `emoji` 选项（emoji 列表）更换表情，通过 `delay` 选项调整表情出现间隔（默认 `16`）。

```js
new cursoreffects.emojiCursor({ emoji: ["🔥", "🐬", "🦆"], delay: 25 });
```

### textFlag

可通过 `text` 选项（字符串）更换 `textFlag` 的文字，通过 `color` 选项（十六进制）更换文字颜色。

```js
new textFlag({ text: "test", color: ["#FF6800"] });
```

## 无障碍

若用户系统开启了 [prefers-reduced-motion](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion) 无障碍设置，光标特效将不会显示。

### trailingCursor

可通过 `particles` 选项（数字）调整 `trailingCursor` 的拖尾步数，通过 `rate` 选项（0 到 1 之间的数字，默认 `0.4`）调整拖尾速率，通过 `baseImageSrc` 选项（URL 或 base64 字符串）更换拖尾光标图片。

```js
new cursoreffects.trailingCursor({
  particles: 15,
  rate: 0.8,
  baseImageSrc: "data:image/png;base64,iVB...",
});
```

可通过 `color` 选项（十六进制）调整 `followingDotCursor` 跟随圆点的颜色。

```js
new cursoreffects.followingDotCursor({ color: ["#323232a6"] });
```

### characterCursor

可把它看作雪花光标的扩展：不用雪花 emoji，而是指定字符列表和颜色，并定义字符在生命周期内速度、旋转和缩放的变化方式。例如，要复现演示页上的效果，可以这样写。（默认也会如此，但这是试验和玩转该效果的好方式。）

```js
new cursoreffects.characterCursor({
  element: document.querySelector("#character"),
  characters: ["h", "e", "l", "l", "o"],
  font: "15px serif",
  colors: ["#6622CC", "#A755C2", "#B07C9E", "#B59194", "#D2A1B8"],
  characterLifeSpanFunction: function () {
    return Math.floor(Math.random() * 60 + 80);
  },
  initialCharacterVelocityFunction: function () {
    return {
      x: (Math.random() < 0.5 ? -1 : 1) * Math.random() * 5,
      y: (Math.random() < 0.5 ? -1 : 1) * Math.random() * 5,
    };
  },
  characterVelocityChangeFunctions: {
    x_func: function (age, lifeSpan) {
      return (Math.random() < 0.5 ? -1 : 1) / 30;
    },
    y_func: function (age, lifeSpan) {
      return (Math.random() < 0.5 ? -1 : 1) / 15;
    },
  },
  characterScalingFunction: function (age, lifeSpan) {
    let lifeLeft = lifeSpan - age;
    return Math.max((lifeLeft / lifeSpan) * 2, 0);
  },
  characterNewRotationDegreesFunction: function (age, lifeSpan) {
    let lifeLeft = lifeSpan - age;
    console.log(age, lifeSpan);
    return lifeLeft / 5;
  },
});
```

注意：这些行为相关的选项都不是必需的；若不提供，将使用与雪花类似的物理效果，并以星号字符代替。

# 许可证

MIT。若你在使用这些脚本，欢迎 [GitHub 赞助](https://github.com/sponsors/tholman) 或 [请我喝杯咖啡](https://www.buymeacoffee.com/tholman) :)
