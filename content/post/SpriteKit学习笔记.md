---
title: "SpriteKit学习笔记"
date: 2020-02-22T11:14:18+08:00
categories: ["SpriteKit"]
draft: false
---

## 1、Sprites

### scene

scene一般代表一个游戏界面。SpriteKit中所有展示在屏幕上的元素都是继承自SKNode

```
import SpriteKit

class GameScene: SKScene {
		// SpriteKit calls before it presents this scene in a view
    override func didMove(to view: SKView) {
        backgroundColor = SKColor.black
        let background = SKSpriteNode(imageNamed: "background")
        background.position = CGPoint(x: size.width / 2, y: size.height / 2)
        background.anchorPoint = CGPoint.zero
        // Sprites are rotated about their anchor points.
        background.zRotation = CGFloat.pi / 8
        addChild(background)
    }
}
```

在游戏中依然使用UIViewController来对游戏进行控制

```
import UIKit
import SpriteKit

class GameViewController: UIViewController {
		override func viewDidLoad() {
				super.viewDidLoad()
				let scene = GameScene(size: CGSize(width: 2048, height: 1536))
				let skView = view as! SKView
				skView.showsFPS = true
				skView.showsNodeCount = true
				skView.ignoreSiblingOrder = true
				scene.scaleMode = .aspectFill
				skView.presentScene(scene)
		}
}
		
```

### Nodes and z-position

每一个node都有一个zPositon属性，默认值是0。每一个Node按照zPositon的顺序从低到高来绘制它的子nodes。

如果ignoresSiblingOrder是true的话，对于zPosition相同的node，SpriteKit不保证它们的绘制顺序，也就是说，可能会存在遮挡现象。

如果ignoresSiblingOrder是false的话，对于相同zPosition的nodes，它们的父node会按照它们的添加顺序来绘制它们。也就是依据addChild方法的书写顺序。

通常情况下，建议将ignoresSiblingOrder设置为ture，这将允许SpriteKit执行性能优化来使游戏的运行速度更快。但是，为了避免遮挡情况的出现，将最底层的node始终置于最下面，可以将该node的zPositon置为-1.如下所示：

`background.zPositon = -1`

### 小结：Adding a sprite to a scene

1. Create the sprite.
2. Postion the sprite.
3. Optionally set the sprite's zPostion
4. Add the sprite to the scene graph.

## 2、Manual Movement

有两种方式可以让sprite移动：

1. actions.
2. set the position manually over time.
### The SpriteKit game loop
In each frame, SpriteKit does the following:
1. Calls a method on your scene named `update(_:)`. This is where you can put code that you want to run every frame - making it the perfect spot for code that updates the position or rotation of your sprites.
2. Does some other stuff.
3. Renders the scene. SpriteKit then draws all of the objects that are in your scene graph, issuing OpenGL draw commands for you behind the scenes.

Here are two tips to keep your game running fase:
1. Keep `update(_:)` fast. For example, you want to avoid slow algorithms in this method since it's called each frame.
2. Keep your node count as low as possible. For example, it's good to remove nodes from the scene graph when they're offscreen and you no longer need them.