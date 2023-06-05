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
### 开发推荐：context->setContextProperty("CommonMsg", &commonMsg);推荐使用引用传参，而非指针传参，避免在QML里面修改commonMsg对象属性值

## 方式二：信号和槽
### 使用场景一：用户通过QML前端界面输入数据，输入数据交由后端进行处理和业务逻辑。（例如：用户通过前端登录界面输入用户名、密码，用户名和密码需要传递给后端，后端读取数据库用户数据进行用户名密码检查，来判断本次登录是否有效）
#### 后端代码示例：
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

signals:
    void loginResult(const QString &new_name);
public slots:
    void sendUsernameAndPassword(const QString &username, const QString &password);
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

void CommonMsg::sendUsernameAndPassword(const QString &username, const QString &password){
    qDebug() << "后端收到用户名：" << username << "，密码：" << password;
}
```
（3）main.cpp
```c++
qmlRegisterType<CommonMsg>("UGroundControl.CommonMsg", 1, 0, "CommonMsg");
```
#### 前端代码示例：
```c++
import UGroundControl.CommonMsg 1.0
CommonMsg {
    id: commonMsg
}
onClicked: {
    commonMsg.sendUsernameAndPassword("zhangsan", "123456")
}
```
### 使用场景二：基于场景一，当后端进行用户名密码检查后，将返回结果通知给前端，前端再通过UI元素展示给用户（例如：提示弹框等）
#### 后端代码示例：
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

signals:
    void loginResult(const QString &result);
public slots:
    void sendUsernameAndPassword(const QString &username, const QString &password);
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

void CommonMsg::sendUsernameAndPassword(const QString &username, const QString &password){
    qDebug() << "后端收到用户名：" << username << "，密码：" << password;
    // 登录业务逻辑
    if(username == "zhangsan" && password == "123456"){
        // 通知登录结果
        emit loginResult("成功");
    }else{
        emit loginResult("用户名密码错误");
    }
}
```
（3）main.cpp
```c++
qmlRegisterType<CommonMsg>("UGroundControl.CommonMsg", 1, 0, "CommonMsg");
```
#### 前端代码示例：
```c++
import UGroundControl.CommonMsg 1.0
CommonMsg {
    id: commonMsg
    onLoginResult: function(data){
        console.log("登录结果：" + data)
    }
}
onClicked: {
    commonMsg.sendUsernameAndPassword("zhangsan", "123456")
}
```
