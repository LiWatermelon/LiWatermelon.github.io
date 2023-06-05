# QT6.5 Quick QML和C++交互

## 术语约定
### 前端：特指QML文件，仅负责UI元素渲染，不负责数据处理与业务逻辑
### 后端：特指C++文件（.h/.cpp），仅负责数据处理与业务逻辑，不负责UI元素渲染

## 方式一：设置上下文信息
### 使用场景：APP启动时，例如用户配置信息或全局通用数据等，由后端加载（从数据库、网络等），QML读取数据进行展示。
### 后端代码示例：
（1）CommonMsg.h
```c++
#ifndef COMMONMSG_H
#define COMMONMSG_H
#include <QObject>
#include "stdafx.h"
class CommonMsg : public QObject
{
    Q_OBJECT
    Q_PROPERTY_AUTO(QString,m_name)
public:
    explicit CommonMsg(QObject *parent = nullptr);
};
#endif // COMMONMSG_H
```
（2）CommonMsg.cpp
```c++
#include "back/CommonMsg.h"
#include <QDebug>

CommonMsg::CommonMsg(QObject *parent)
    : QObject{parent}
{
    this->m_name("初始值");
}
```
（3）main.cpp
```c++
QQmlApplicationEngine engine;
QQmlContext *context = engine.rootContext();
CommonMsg commonMsg; //通用数据
context->setContextProperty("CommonMsg", &commonMsg);
```
### 前端代码示例：
```c++
Text{
  text: CommonMsg.m_name
}
```
## 方式二：设置上下文信息
### 使用场景：APP启动时，例如用户配置信息或全局通用数据等，由后端加载（从数据库、网络等），QML读取数据进行展示。

