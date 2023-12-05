---
title: TypeScript开发游戏学习笔记
date: 2022-05-21 10:15:56
tags:
  - TypeScript
  - 游戏开发
  - 前端开发
  - 编程语言
  - Web开发
  - 游戏设计
  - 前端框架
  - 工具
  - 学习笔记
  - 教程
  - 杆踪球影冰球游戏
categories:
- 笔记
---

### “烂码”

看之前写的杆踪球影中NPC这段代码可能确实存在一些潜在的问题。其中可能比较明显的是，NPC在Chapter.Total状态下的行为过于复杂，导致代码变得比较混乱难以维护。

此外，NPC移动的速度也受到了限制，可能会导致玩家在进行游戏时感到不自然。现在觉得最好的做法是将代码进行重构，尽量将每个状态下的行为分开处理，并考虑到玩家在游戏中的体验。
<!-- more -->

### 截取某段

```typescript
else if (this.stihs.chapter === Chapter.Total) {
  if (this === this.stihs.redCharacters[5] || this === this.stihs.blueCharacters[5]) {
    if (!this.locked) {
      if (!this.isAutoMoving) {
        const pos = this.node.getPosition();
        const rand = 1 - 2 * randomRangeInt(0, 2);
        pos.x += rand * 0.5;
        if (-2.5 <= pos.x && pos.x <=2.5) {
          this.isAutoMoving = true;
          tween(this.node)
          .to(0.5, { position: pos }, { easing: "linear" })
          .call(() => {
            this.isAutoMoving = false;
          })
          .start();
        }
      }
    }
  } else {
    if (!this.locked) {
      if (this.stihs.player?.gamePad) {
        if (this.stihs.player.gamePad.dir.length() > 0.5) {
          const skeletalAnimation = this.getComponent(
            SkeletalAnimation
          ) as SkeletalAnimation;
          const animState1 = skeletalAnimation.getState("起步");
          const animState2 = skeletalAnimation.getState("溜冰");
          const animState3 = skeletalAnimation.getState("急停");
          if (!animState1.isPlaying && !animState2.isPlaying) {
            skeletalAnimation.play("起步");
          }
          const rigidBody = this.getComponent(RigidBody) as RigidBody;
          let { x, y } = this.stihs.player.gamePad.dir;
          let angle = Math.atan2(y, x);
          const delta = Math.random() * Math.PI - Math.PI / 2;
          angle += delta;
          [x, y] = [Math.cos(angle), Math.sin(angle)];
          rigidBody.applyForce(
            new Vec3(
              x * 5000 * deltaTime,
              0,
              y * -5000 * deltaTime
            )
          );
          let v: Vec3 = new Vec3(0, 0, 0);
          rigidBody.getLinearVelocity(v);
          let vLen = v.length();
          if (vLen > 7) {
            v.divide(
              new Vec3(1, 1, 1).multiplyScalar(vLen)
            ).multiplyScalar(7);
            vLen = 7;
            rigidBody.setLinearVelocity(v);
            if (animState1.isPlaying) {
              animState1.stop();
              skeletalAnimation.crossFade("溜冰", 0.3);
              animState2.speed = 1;
            }
          }
          if (!this.facedTarget) {
            this.dir = new Vec3(v.x / vLen, v.y / vLen, v.z / vLen);
          }
        } else {
          const skeletalAnimation = this.getComponent(
            SkeletalAnimation
          ) as SkeletalAnimation;
          const animState1 = skeletalAnimation.getState("起步");
          const animState2 = skeletalAnimation.getState("溜冰");
          const animState3 = skeletalAnimation.getState("急停");
          if (animState1.isPlaying) {
            animState1.setTime(0);
            animState1.stop();
          }
          if (animState2.isPlaying) {
            skeletalAnimation.play("急停");
            animState3.repeatCount = 1;
          }
        }
      }
    }
  }
}

```

### 可以使用曲线函数调整NPC的移动速度：

```typescript
phpCopy code
// 定义速度曲线函数
const speedCurve = new cc.AnimationCurve([
  { time: 0, value: 0 },
  { time: 0.25, value: 0.5 },
  { time: 0.5, value: 1 },
  { time: 0.75, value: 0.5 },
  { time: 1, value: 0 }
]);

// 在 update 方法中使用曲线函数调整移动速度
update(deltaTime: number) {
  if (this.isAutoMoving && this.stihs) {
    const speed = speedCurve.evaluate(this.elapsedTime / this.moveDuration) * this.maxSpeed;
    // 使用 speed 进行移动
  }
}
```

### 再添加物理效果：

```typescript
kotlinCopy code
// 定义 NPC 的物理属性
const mass = 1;
const friction = 0.5;
const restitution = 0.2;

// 在初始化方法中为 NPC 添加刚体组件
onLoad() {
  const rigidbody = this.node.addComponent(cc.RigidBody);
  rigidbody.type = cc.RigidBodyType.Dynamic;
  rigidbody.gravityScale = 0;
  rigidbody.mass = mass;
  rigidbody.linearDamping = friction;
  rigidbody.angularDamping = friction;
  rigidbody.restitution = restitution;
}

// 在 update 方法中更新 NPC 的位置
update(deltaTime: number) {
  if (this.isAutoMoving && this.stihs) {
    const velocity = cc.v2(this.moveDirection).normalize().mul(this.maxSpeed);
    const force = velocity.mul(mass);
    const rigidbody = this.node.getComponent(cc.RigidBody);
    rigidbody.applyForceToCenter(force);
  }
}
```

### 然后解决在Chapter.Total状态下的行为过于复杂，导致代码变得比较混乱难以维护。

直接继承 `Chapter.Shoot` 状态下的行为,将 `Chapter.Shoot` 状态下的行为提取成一个单独的方法或者函数，并将其放在一个可重用的地方。然后在需要使用这个行为的地方，调用该方法或函数即可。

```typescript
kotlinCopy code
class NPC extends Character {
  private isAutoMoving: boolean = false;
  private stihs: Stihs | null = null;

  // ...

  private moveRandomlyWithinRange(range: number) {
    // 移动到随机的位置
  }

  private playSkatingAnimation(speed: number) {
    // 播放溜冰动画
  }

  private updateBlockChapter() {
    this.playSkatingAnimation(1.5); // 播放溜冰动画，速度为 1.5
  }

  private updateShootChapter() {
    if (!this.isAutoMoving) {
      this.isAutoMoving = true;
      this.moveRandomlyWithinRange(10); // 在范围内随机移动
    }
  }

  private updateTotalChapter() {
    if (this instanceof SpecificNPC) {
      this.updateShootChapter(); // 直接继承 Chapter.Shoot 状态下的行为
    } else {
      // 移动方向由玩家游戏手柄输入的方向确定，加上一些小的随机偏差
      // 移动速度受到限制，超过一定速度会切换到溜冰动画
      this.playSkatingAnimation(1.2);
      // ...
    }
  }

  public update() {
    if (this.stihs === null || this.isLocked) {
      return;
    }

    switch (this.stihs.currentChapter) {
      case Chapter.Block:
        this.updateBlockChapter();
        break;
      case Chapter.Shoot:
        this.updateShootChapter();
        break;
      case Chapter.Total:
        this.updateTotalChapter();
        break;
    }
  }
}
```

到此差不多了
