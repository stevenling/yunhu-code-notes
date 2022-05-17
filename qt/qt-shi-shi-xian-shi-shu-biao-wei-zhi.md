# Qt 实时显示鼠标位置

### 一、概述

想要实时显示当前鼠标的位置。

### 二、步骤

#### 2.1 重写 event 事件

```cpp
bool event(QEvent *event)
{
    // 事件类型是移动鼠标
    if (event->type() == QEvent::MouseMove)
    {
        QMouseEvent *mouseEvent = static_cast<QMouseEvent *>(event);
        QPointF p = mouseEvent->pos(); // 获取鼠标位置
        QPoint currentMousePoint = p.toPoint();
        emit SendCurrentMousePoint(currentMousePoint); // 发送信号到主界面
    }
    return QWidget::event(event);
}
```

#### 2.2 关联信号和槽

设置信号和槽，发送当前的鼠标位置，槽函数接收到鼠标位置后，显示到控件上即可。
