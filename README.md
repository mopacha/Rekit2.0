
# Rekit 2.0 构建基于React+Redux+React-router的可扩展Web应用

 <img src="http://rekit.js.org/images/logo_text_2.0.png?raw=true?raw=true" width="300"> 

> [Rekit 2.0 出了很棒的新功能!](https://medium.com/@nate_wang/rekit-2-0-next-generation-react-development-ce8bbba51e94)
>
> 文章翻译来自：[http://rekit.js.org/](http://rekit.js.org/)
>
> 本文翻译未经授权不得转载或转载请注明出处

## 前言

**Rekit**—用于创建基于 React+Redux+React-router的可扩展Web应用，它是用于创建React app的全功能解决方案。

它可以帮助您只需专注于业务逻辑，而不是处理大量的库、模式、配置等。

Rekit创建应用程序遵循一般的最佳实践，创建的应用程序具有可扩展性、可测试性和可维护性，优化了应用程序逻辑的归类和解耦。

除了创建应用程序之外，Rekit还提供了强大的工具来管理项目：

 **1. 命令行工具**: 您可以使用这些工具来创建/重命名/移动/删除项目元素，比如components、 actions等。

 **2. Rekit portal**:它是一个新的开发工具，附带了Rekit 2.0。它不仅提供了用于创建/重命名/移动/删除Rekit应用的web UI，而且还提供了用于分析/构建/测试Rekit应用的测试工具，你可以将Rekit portal视为React开发的IDE。参见在线演示： [https://rekit-portal.herokuapp.com](https://rekit-portal.herokuapp.com)


下面是两个快速视频演示(需要翻墙):

1. [计数器](https://youtu.be/HT6YzZtbPKc): 花费1分钟创建一个简单计数器！

[<img src="http://rekit.js.org/images/video-counter.png" width="400" alt="Rekit Demo"/>](https://youtu.be/HT6YzZtbPKc)

2. [Reddit API的用法](https://youtu.be/edq5lWsxZMg): 在Reddit上使用异步actions来显示最新的`react.js`主题.

[<img src="http://rekit.js.org/images/video-reddit-api.png" width="400" alt="Rekit Demo"/>](https://youtu.be/edq5lWsxZMg)

## 入门

尝试使用Rekit最简单的方式是创建一个Rekit APP并且玩转它，仅仅3步：

**1. 安装Rekit**

    npm install -g rekit

**2. 创建app**

    $ rekit create my-app
    $ cd my-app
    $ npm install

**3. 运行**

    $ npm start

### 进入welcome page

应用程序应该在几秒钟内启动，你可以输入下面URL访问：[http://localhost:6075](http://localhost:6075)

如果一切正常，您应该能够看到如下的welcome page：

<img src="http://rekit.js.org/images/welcome-page.png" alt="Rekit Demo"/>

页面由3部分组成：

**1. 一个简单的导航组件**。它读取整个应用程序的路由配置，并生成指向不同页面的链接。

**2. 使用同步actions的计数器演示**。通过示例，您可以快速地看到component、actions和reducers三者是怎样协同工作的。

**3. 通过Reddit来演示获取`reactjs`最新主题**。这仅仅是来自官方Redux网站的例子：
 [https://redux.js.org/docs/advanced/ExampleRedditAPI.html.](https://redux.js.org/docs/advanced/ExampleRedditAPI.html.)
它体现了Redux应用的[异步actions](http://rekit.js.org/docs/concepts#async-action)。但Rekit版本`one action one file pattern`(下面章节会介绍)，并增加了错误处理，这是在应用程序的实践开发中共同的要求。

### 试用Rekit portal

Rekit portal是装载在Rekit 2.0上的全新开发工具。当一个Rekit APP启动时，Rekit portal也会自动启动，本地默认启动地址为：[http://localhost:6076](http://localhost:6076)

<img src="http://rekit.js.org/images/portal-local.png" alt="Rekit portal"/>

它不仅提供了用于创建/重命名/移动/删除Rekit APP元素的Web UI，而且还提供了用于分析/构建/测试Rekit应用的许多测试工具。

从网页面板中，您发现尚未生成的测试覆盖报告，不要犹豫，点击运行测试按钮，你会快速的发现Rekit portal是怎样用超级简单的方式做这些工作的。

有关Rekit portal的更多介绍，在本文的后面章节会做出详细阐述。

### 何处着手

Rekit会默认创建一个SPA，您可以根据需要编辑根容器来定义自己的容器布局，源文件位于`src/features/home/App.js`。

### 两个快速教程视频

在welcome page有两个实例，它们也是Redux官方网站的演示。现在让我们看看如何用Rekit创建它们。

1. [一分钟用Rekit创建一个计数器](https://youtu.be/HT6YzZtbPKc)

[<img src="http://rekit.js.org/images/youtube.png" alt="couter"/>](https://youtu.be/HT6YzZtbPKc)

2. [创建一个Reddit列表（5分钟）。](https://youtu.be/edq5lWsxZMg)

[<img src="http://rekit.js.org/images/youtube.png" alt=""/>](https://youtu.be/edq5lWsxZMg)

### 就这样

你已经创建了你的第一Rekit APP，并且尝试了强大的Rekit工具。现在你可以在下面几页阅读到更多的有关Rekit细节。

## Rekit app 架构

通过Rekit，仅仅使用一行命令就可以创建一个React app, 而不需要其它的额外配置。该应用程序的设计具有可扩展性、可测试性和可维护性，以满足实际应用程序的要求。

Rekit一个关键理念是将一个大的应用程序划分为特性片段，我们把它称为features，每个feature是应用程序的某个功能特性，它体积小、解耦性好，易于理解和维护。  

一个React App 已经自然的由组件树构建UI。实际上通过结合Redux reducers，定义React router配置的子路由，我们也可以把整个应用程序的store，路由配置成小块。通过这样做，我们可以将可以把复杂应用程序的管理变成对应用块的管理，请参阅下图，以了解整个Rekit应用程序架构。     
  
[<img src="http://rekit.js.org/images/app-architecture.png" alt=""/>]()

每个feature是一个小的`app`，这很容易理解，一个大的应用程序由许多这样的feature组成。

### 目录结构

如上图所示，Rekit使用一个特殊的文件夹结构创建一个应用程序。根据features将应用程序逻辑分组，每个feature都包含自己的components、actions、路由配置等。

```
|-- project-name
|    |-- src
|    |    +-- common
|    |    |-- features
|    |    |    |-- home
|    |    |    |    +-- redux
|    |    |    |    |-- index.js
|    |    |    |    |-- DefaultPage.js
|    |    |    |    |-- DefaultPage.less
|    |    |    |    |-- route.js
|    |    |    |    |-- styles.less
|    |    |    |    |-- ...
|    |    |    +-- feature-1
|    |    |    +-- feature-2
|    |    +-- styles
|    --- tools
|    |    +-- plugins
|    |    |-- server.js
|    |    |-- build.js
|    |    |-- ...
|-- .eslintrc
|-- .gitignore
|-- webpack-config.js
|-- ...
```

### 概念

Rekit不封装或更改任何React、Redux和React router 的API，只根据最佳实践来创建应用程序，并提供管理项目的工具。因此，项目中没有新的概念（也许除了feature之外），如果你能理解React, Redux 和 React router，你能够很容易的理解Rekit项目。

下面介绍下这些概念是如何被Rekit管理和使用的。

### Feature

Feature是项目的顶层概念，它是Rekit的核心理念，一个feature实际上是描述应用程序某些功能的一种自然方式。例如，一个电子商店应用程序通常包含以下功能：

* customer 管理基本客户信息。
* product: 销售管理产品。
* category: 管理产品类别。
* order:管理销售订单。
* user: 系统管理员管理。
* etc...

一个feature 通常包含多个actions、组件或路由规则，一个Rekit应用总是由多个feature组成。通过种方法，一个大的应用程序可以被划分为多个小的、完全解耦的、易于理解的features。

当创建Rekit应用时，会自动生成两个默认的features。

**1. common**：它是放置所有跨feature元素（如components,actions,等)的地方。在Rekit1.0版本中，有一个单独的components目录来用来存放common components。React2.0版本中。我们把它们放在一个common feature目录中，通过这个优化，减少了conepts的数量，使目录结构更加简单一致。

**2. home**:项目的基本feature和应用程序的起点，通常把最基本的功能放在这里，如整体布局容器，基本应用程序逻辑等。

然而这仅仅是Rekit推荐的方式，如果您愿意，你可以重命名目录或删除默认功能。

为了快速领悟feature概念，你可以看看Rekit portal的在线演示：

[https://rekit-portal.herokuapp.com](https://rekit-portal.herokuapp.com.)

查看更多有关feature的介绍：[ feature oriented architecture](http://rekit.js.org/docs/feature-oriented-architecture)
 
### Component

根据[Redux理论](http://redux.js.org/docs/basics/UsageWithReact.html)，组件（components）可以分为两类：容器型组件(`container`)和展示型组件(`presentational`)。Rekit可以很容易的创建它们。

    rekit add component home/Comp [-c]

`-c`标志表明它是一个容器型组件，否则是一个展示型组件。Rekit将使用不同的模板生成相应的组件。

为了能够使用React router，组件是一些URL模式的代表，Rekit允许指定`-u`参数：

    rekit add component home/Page1 -u page1

这将在feature目录的`router.js`文件中定义一个路由规则，然后你可以在本地输入： [http://localhost:6075/home/page1](http://localhost:6075/home/page1) 来访问组件。


### Action

说的是[Redux action](http://redux.js.org/docs/basics/Actions.html)。有两种类型的actions：同步(`sync`)和异步(`async`)。正如Redux的文档所描述的，异步action实际上不是一个新概念，而是提出了异步操作的数据和工作流程。

默认情况下，Rekit使用`Redux thunk`的`async actions`。当创建一个`async-action`时，Rekit创建代码样板来处理请求的开始，请求等待，请求成功，请求失败action类型。在reducer中维护`requestpending`，`requestError`状态。使用下面的命令行工具，它会自动创建一个async操作的样板文件，只需要在不同的技术构件中填充应用程序逻辑:即可。

    rekit add action home/doRequest [-a]

`-a`标志表明它是否是一个异步的action，否则同步action。

或者，你可以安装插件[rekit-plugin-redux-saga](https://github.com/supnate/rekit-plugin-redux-saga)用[redux-saga](https://github.com/redux-saga/redux-saga)创建异步actions。

### Reducer

这里讲的是Redux reducer。Rekit会按照官方方式为reducers重新组织代码结构。可以阅读下面章节的one action one file 来获取更多的介绍。

### One Action One File

这可能是Rekit方法中最具自主性的部分，也就是：` one action one file`，并且把相应的reducer放在同一个文件中。

这个想法来自Redux开发所带来的痛点：它几乎总是在创建一个新的aciton后，写一个reducer。

就拿计数器组件的例子来说，在创建新的action `COUNTER_PLUS_ONE`后，我们立即需要在reducer中来处理它，官方的做法是将代码分开，分别写在actions.js和reducers.js两个文件中。在新的方法中，我们创建一个名为`counterPlusOne.js`的新文件，把下面的代码放入里面。

```
import {
  COUNTER_PLUS_ONE,
} from './constants';

export function counterPlusOne() {
  return {
    type: COUNTER_PLUS_ONE,
  };
}

export function reducer(state, action) {
  switch (action.type) {
    case COUNTER_PLUS_ONE:
      return {
        ...state,
        count: state.count + 1,
      };

    default:
      return state;
  }
}
```

根据我的经验，大多数reducers 都有相应的actions，它很少在全局中使用。因此，将其放在一个文件中是合理的，且使开发更容易。

这里的reducer不是一个标准的Redux reducer，因为它没有一个初始状态。它只用在feature的根reducer，它通常被称为`reducer`。这样，根reducer就可以从action 模块自动加载它。

对于异步actions，action文件可以包含多个action，因为它需要处理错误。对于Rekit应用，每个feature都包含一个命名为`redux`的文件夹，在这个文件夹中放置actions, constants 和 reducers。


### 如何跨主题actions？

虽然不是很多，但有需要跨主题action的应用场景。例如，当收到新消息：

* 如果聊天室打开，显示消息。
* 如果没有，显示通知图标/消息。`NEW_MESSAGE `action需要不同的UI组件处理。因此，每个feature都有一个根reducer`<feature-name>/redux/reducer.js`,在这里你写的就是何跨主题reducers。

### feature的根reducer是如何工作的？

虽然不同的`reducers`被分离成不同的文件，但是它们运行相同的状态，也就是Redux store的同一个分支。因此，一个 feature reducer 有下面的代码模式：

```
import initialState from './initialState';
import { reducer as counterPlusOne } from './counterPlusOne';
import { reducer as counterMinusOne } from './counterMinusOne';
import { reducer as resetCounter } from './resetCounter';

const reducers = [
  counterPlusOne,
  counterMinusOne,
  resetCounter,
];

export default function reducer(state = initialState, action) {
  let newState;
  switch (action.type) {
    // Put global reducers here
    default:
      newState = state;
      break;
  }
  return reducers.reduce((s, r) => r(s, action), newState);
}
```

使用Rekit创建actions，你不需要手动维护此文件，因为Rekit将自动更新它以添加或移除reducer导入。

### 益处

这种方法有多种优点：

* 易于开发：创建actions时不需要在文件之间跳转。
* 易于维护：现在actions文件很小，很容易通过文件名找到。
* 易于测试：一个测试文件对应一个action，还包括action和reducer测试。
* 易写的工具：当创建一个工具来生成Redux样板代码时不需要解析代码，一个文件模板就足够了。
* 易于分析：通过静态分析可以很容易地找到跨主题actions。

### 命名规范

Rekit通过自动转换输入来强制达到一致的命名规则。无论是命令行工具或Rekit portal都要遵循以下命名规则来创建features、 components 和 actions，如果手动创建它们，也应该遵循这些规则。

* `feature`：文件夹名称： `kebab case( 短横线隔开)`。例如：`rekit add feature myFeature`将创建一个文件夹，命名为`my-feature`。
* `redux store` :驼峰拼写法。当添加一个feature时，Rekit将把 feature reduce合并到根reducer,并以驼峰式命名作为分支名称。
* `url path`: 短横线隔开。它将把`-u MyPage`参数修改映射到一个页面。对于这个命令，它将URL路径定义为路由配置中的`my-page`。
* `component`:文件名和样式文件名：驼峰式大小写。例如：`rekit add component feature/my-component`将创建文件`MyComponent.js`和`MyComponent.less`。
* `action`: 函数名: 驼峰拼写法. 例如: `rekit add action feature/my-action` 将会在actions.js中创建一个名为 `myAction`的函数 。
* `action type`: 常量名称和值：`upper snake case`。Action types是在创建action时创建的。例如:`rekit add action home/my-action` 将会创建一个 action type `HOME_MY_ACTION`。

如您所见，任何作为参数的名称都将被转换。因此，Rekit应用程序的所有变量都是一致的，易于理解。

## Rekit core

Rekit core提供了用于管理Rekit应用的核心功能，常被用在Rekit命令行工具和Rekit portal中。

当一个Rekit应用程序被创建时，它会自动添加`rekit-core`作为一个依赖项。当您执行`rekit add feature f1`这样的命令时，它会找到当前安装的`rekit-core`，在项目中添加了一个`feature`。因此，`rekit-core`不是全局性的，而是独立于项目的。不同的Rekit应用程序可以使用不同版本的`rekit-core`。保证了其它项目升级`rekit-core`时不会破坏现有的Rekit应用程序。

### API 参考

Rekit core APIs 有着良好地模块化和文档化，是创建定制Rekit插件的基础。

您可以查看API文档:[http://rekit.js.org/api](http://rekit.js.org/api)。

您可以根据`rekit-core`创建自己的插件。

## Rekit portal

Rekit portal是一个新的开发工具，附带了Rekit 2.0，它在管理和分析您的Rekit项目中占有核心地位。Rekit portal本身也是由Rekit创建的，因此，它也是学习Rekit所参考的一个很好的例子。

为了快速查看Rekit portal是如何工作的，您可以查看[在线演示](https://rekit-portal.herokuapp.com/)。


### 关键features

* 提供一种更直观的方式来创建、重命名、移动或删除features、components或actions，而不是CLI，就如同使用eclipse这样的IDE创建Java类一样。

* 通过源代码生成项目体系结构的图表报告。因此，新团队成员或者你自己，很短时间内就能够上手项目。

* 只需右键单击，就可以轻松运行测试单个组件或action。

* 不用打开终端就可运行构建。

* 集成测试覆盖报告。

### 安装

不需要手动安装Rekit portal，当创建一个新的Rekit应用程序时，`rekit-portal`将自动依赖于npm模块。打开[http://localhost:6076](http://localhost:6076)即可访问Rekit portal。

### 项目资源管理器

项目资源管理器通过将源文件按`features`、`actions`、`components`分组来提供更有意义的项目文件夹结构视图。您可以很容易地看到功能结构，而不仅仅是文件夹结构，您可以在Rekit portal的左侧看到它:

[<img src="http://rekit.js.org/images/portal-project-explorer.png" width='300' alt="project-explorer"/>]()

除了显示项目结构之外，它还提供了一些上下文菜单来管理诸如`components`之类的项目元素。

### 显示面板

显示面板提供了项目的总体状态视图，如概览图，测试覆盖率等。

[<img src="http://rekit.js.org/images/portal-dashboard.png" width="700" alt="dashboard"/>]()

### 概览图

显示面板中最引人注目的部分是概览图。这是一个关于Rekit项目架构的直观视图。它具有交互性，您可以把鼠标移动到features、 components或、actions上，来查看某些特定元素的关系。您还可以单击一个节点深入其中，下面的信息被概览图所涵盖:

1. 模块之间的关系。

3. features的相对大小。

4. 一个feature是如何组成的。

当鼠标经过一个元素时，图表将突出显示当前元素和与当前元素有关的关系。

理想情况下，不应该在features之间存在循环依赖。所以它们是可插入的并且更容易理解。但是在实际项目中，您需要平衡架构和开发效率。因此，如果在features之间有轻量级的循环依赖关系，而原则是避免太多此类依赖关系，这是可以接受的。当某些类型的依赖关系变得过于复杂时，您可以延迟删除依赖项进行重构。

这里列出了不同颜色和线条的含义:

[<img src="https://user-images.githubusercontent.com/20238205/28239312-309c3d70-699c-11e7-9e5c-d33d88bde71d.png" width="700" alt="dashboard"/>]()

### 元素图

虽然概览图展示了项目的总体架构，但元素图提供了所选元素和其他元素之间的关系，它有助于快速理解一个模块，并帮助找出过于复杂的模块。

当您从项目资源管理器或概览图中单击一个元素时，它将默认显示元素图:

<img src="http://rekit.js.org/images/element-diagram.png" width="550" alt="element-diagram">

### 测试覆盖率

Rekit使用[istanbul](https://github.com/gotwarlost/istanbul)生成测试覆盖率报告。在对项目运行所有测试之后，测试覆盖率将有效。运行单个测试或文件夹的测试将不会生成覆盖率报告。注意，如果一些测试失败，报告数据可能是不完整的。

您可以看到来自显示面板的总体测试覆盖率报告，或者来自详细页面的由[istanbul-nyc](istanbul-nyc)生成的原始测试覆盖率报告。

### 管理项目的元素

Rekit portal将命令行工具封装到UI对话框中，可以直观地创建、重命名或删除components、actions等。打开对话框，右键单击项目中的某个元素，然后单击相应的菜单项。

<img src="http://rekit.js.org/images/cmd-dialogs.png" width="700" alt="cmd-dialogs">

### 运行构建

Rekit portal执行构建脚本`tools/build.js`。当单击菜单项` Build`时，它将读取Webpack构建进度数据来更新进度条。尽管`build.js`是由Rekit创建的，看起来有点复杂，但是在完全理解它的工作原理之后，您可以根据需求更新它。

<img src="http://rekit.js.org/images/portal-build.png" width="550" alt="build">

### 运行测试

Rekit portal执行测试脚本`tools/run_test.js`。当单击菜单项`Run tests`时，脚本接受测试运行的参数，参数可以是单个文件或文件夹。当没有参数提供时，它运行`tests`文件夹下的所有测试，并生成测试覆盖率报告。在下面章节的命令行工具页的介绍中可以查看到更多详细信息。

因此，当单击项目元素组件上的`Run test`菜单项时，它只执行`tools/run_test.js`，将当前的组件测试文件作为参数传递给脚本。您还可以根据需要来更新你的`run_test.js`脚本。

<img src="http://rekit.js.org/images/portal-test.png" width="550" alt="test">

### 代码查看器

它有助于快速查看项目的源代码。例如，当选择一个组件时，默认情况下它会显示图表视图，但是您可以切换到代码视图，在那里您可以查看组件的源代码。您还可以轻松地查看样式代码或测试文件。目前，Rekit并没有直接支持编辑代码，因为它不打算替换您喜爱的文本编辑器。

<img src="http://rekit.js.org/images/element-page.png" width="700" alt="element-page">

## 命令行工具

Rekit提供了一组命令行工具来管理Rekit项目的 components, actions和路由规则。它们作为JavaScript模块在`rekit-core`中实现，Rekit将它们封装为命令行工具，实际上`rekit-core`也由`Rekit Portal`使用。

### Create an app

您可以使用以下命令创建一个应用程序:

    rekit create <app-name> [--sass]

这将会在当前目录下创建一个名为`app-name`的应用程序。`--sass`标记表明允许使用sass而不是[less](http://lesscss.org/)`来当作css编译器。

### Create a plugin

您可以使用以下命令创建一个插件:

    rekit create-plugin <plugin-name>

如果当前目录是在Rekit项目中，那么将创建一个本地插件，否则创建一个公共插件。

想了解更多信息，请阅读以下文件:[ http://rekit.js.org/docs/plugin.html](http://rekit.js.org/docs/plugin.html)

### 安装一个插件

如果你想通过npm使用一个插件，请使用下面的命令:

    rekit install <plugin-name>

这将执行插件的`install.js`脚本进行初始化，并添加名为`rekit.plugins`的插件到package.json中。

### Rekit tools

Rekit工具是创建的应用程序附带的纯脚本，它们被放入在你应用程序的`tools`文件夹中，并且支持编辑以满足项目的额外要求。

**tools/server.js**

这个脚本用于启动开发服务器，默认情况下，它启动3个服务器，包括：webpack dev server、Rekit portal和 build result server，您只能通过参数启动某个服务器。

用法：

    node tools/server.js [-m, --mode] [--readonly]


* `mode`: 如果没有提供，则启动所有3个开发服务器。否则，只启动指定的开发服务器。它可以是:

    * dev: webpack dev server

    * portal: Rekit portal
    * build: start a static express server for build folder
* `readonly`:在只读模式下启动Rekit portal。启动Rekit portal服务器只用于探索项目结构是很有用的。例如，[Rekit portal演示](https://rekit-portal.herokuapp.com/)是在只读模式上运行。

`npm`脚本也是有效的： `npm start`


**tools/run_test.js**

这个脚本有助于运行一个或多个单元测试。它接受参数来判断哪个测试文件应该运行。

用法：

    node tools/run_test.js <file-pattern>

文件模式与`mocha`所接受的相同。如果没有指定`file-pattern`，则运行所有测试并生成测试覆盖率报告。否则运行测试与`file-pattern`匹配。

例如：

    node tools/run_test.js features/home/redux/counterPlusOne.test.js  // run test of a redux action
    node tools/run_test.js features/home // run all tests of home feature
    node tools/run_test.js // run all tests and generate test coverage report

也可以运行npm脚本: `npm test`.

**tools/build.js**

这个脚本用于构建项目。

用法：

    node tools/build.js

也可以使用`npm`脚本：`npm run build`。它将项目构建到`build`文件夹中。

#### 管理 features, components and actions

这是每日Rekit发展的关键。您将使用以下命令来管理Rekit元素。

> 注意:尽管所有命令都放在`rekit`命令下，也就是`rekit add component home/comp1`。实际上Rekit会在你的应用程序中找到本地的`rekit-core`包来完成运行。因此，如果这些应用程序依赖于不同版本的`rekit-core`，那么在不同的rekit应用程序下执行rekit命令可能会有不同的行为。

所有这些命令都有类似的模式：

* `rekit add <type>`: 添加一个类型的元素。
* `rekit mv <type>`: 移动/重命名一个类型的元素。
* `rekit rm <type>`: 删除一个类型的元素。

所有命令都支持[-h]参数来查看使用帮助。即`rekit add -h`。

下面是所有Rekit命令来管理项目的元素列表

|Commands|Description|
|:---|:---:|---:|
|rekit add feature <feature-name>|Add a new feature.|
|rekit mv feature <old-name> <new-name>|Rename a feature.|
|rekit rm feature <feature-name>|Delete a feature.|
|rekit add component <component-name> [-u] [-c]|Add a new component.`-u`: specify the url pattern of the component. `-c`: it's a container component connected to Redux store.|
|rekit mv component <old-name> <new-name>|Rename a component.|
|rekit rm component <component-name>|Delete a component.|
|rekit add action <action-name> [-a]|Add a new action.  `-u`: add an async action.|
|rekit mv action <old-name> <new-name>|Rename an action.|
|rekit rm action <action-name>|Delete an action.|

## 插件

Rekit 2.0引入了一个新的插件机制来扩展Rekit的功能。如果您尝试过Rekit命令行工具，您可能熟悉它的模式:

    rekit <add|rm|mv> <element-type> <feature>/</element-name>

内部Rekit支持3种元素类型:: `feature`, `component `和 `action` 并且定义了怎么 `add/rm/mv` 它们。

现在，您可以创建一个Rekit插件来改变默认行为，例如Rekit创建异步action或让Rekit支持基于[reselect](https://github.com/reactjs/reselect)的新元素类型`selector`。

实际上，有这样的两个插件:

1. [rekit-plugin-redux-saga](https://github.com/supnate/rekit-plugin-redux-saga): 允许Rekit在创建异步操作时使用`redux-saga`而不是`redux-thunk`。
2. [rekit-plugin-reselect](https://github.com/supnate/rekit-plugin):添加一个基于[reselect](https://github.com/reactjs/reselect)的新元素类型选择器。因此，您可以通过Rekit为您的项目管理选择器。

### 创建一个插件

要创建一个插件，请使用以下命令:
    
    rekit create-plugin <plugin-name>

如果当前目录在Rekit项目中，它将创建一个本地插件，否则创建一个公共插件。

### 插件结构

创建一个插件后，您可以查看文件夹结构。在插件文件夹下可能有一些特殊的文件:

**config.js**

插件唯一的mandotory文件，它定义了处理的元素类型，必要时定义命令参数。例如:

```
module.exports = {
  accept: ['action'],
  defineArgs(addCmd, mvCmd, rmCmd) { // eslint-disable-line
    addCmd.addArgument(['--thunk'], {
      help: 'Use redux-thunk for async actions.',
      action: 'storeTrue',
    });
  },
};
```

有两部分：

**1. accept**

它是一个数组，用来接收插件要处理的类型元素。当一个类型的元素被定义在这里时，应该有一个名为`${ elementType }.js`的模块。在这里定义了添加、删除、移动方法来管理这些元素。例如，元素类型`action`被定义，在插件文件夹里将会有一个`action.js`模块:

```
module.exports = {
  add(feature, name, args) {},
  remove(feature, name, args) {},
  move(source, target, args) {},
};
```

您可以只导出一些`add`、`remove`和`move`。例如，如果只定义`add`命令，那么当执行`rekit mv action ...`时,它将默认回归到Rekit`mv`actions。

您还可以分开创建3个插件`add`、`remove`和`move`，它们接受相同的元素类型。

**2. defineArgs(addCmd, mvCmd, rmCmd)**

Rekit使用[argparse](https://www.npmjs.com/package/argparse)来解析命令参数。这种方法允许为命令行工具自定义参数。这里的`addCmd`、`mvCmd`和`rmCmd`都是全局Rekit命令的子命令。根据argparse的文档，您可以为子命令添加更多的选项以满足您的需求。例如:`redux-saga`插件定义了一个新选项——`thunk`,默认情况下`redux-saga`允许在异步操作中使用`redux-thunk`，而`redux-saga`在默认情况下使用。然后您就可以使用:

    rekit add action home/my-action -a --thunk

**hooks.js**

当您想要挂起某些操作时，此文件仅是必需的。即在创建或删除`feature`之后，做一些事情。每个元素类型有两种钩子点:`before` 和`after`，它们与操作类型相结合，形成多个钩子点。例如，`feature`元素类型有一下钩子点:

- beforeAddFeature()
- afterAddFeature()
- beforeMoveFeature()
- afterMoveFeature()
- beforeRemoveFeature()
- afterRemoveFeature()

参数只从钩子目标继承。也就是说，传递给addFeature的任何参数也都传递到beforeAddFeature()。

注意，不只是内部元素类型有钩子点，所有被插件支持的元素类型都有这样的钩子点。

例如，`redux-saga`插件使用钩子在添加或删除`features`时进行初始化和初始化。

```
const fs = require('fs');
const _ = require('lodash');
const rekitCore = require('rekit-core');

const utils = rekitCore.utils;
const refactor = rekitCore.refactor;
const vio = rekitCore.vio;

function afterAddFeature(featureName) {
  // Summary:
  //  Called after a feature is added. Add sagas.js and add entry in rootSaga.js
  const rootSaga = utils.mapSrcFile('common/rootSaga.js');
  refactor.updateFile(rootSaga, ast => [].concat(
    refactor.addImportFrom(ast, `../features/${_.kebabCase(featureName)}/redux/sagas`, null, null, `${_.camelCase(featureName)}Sagas`),
    refactor.addToArray(ast, 'featureSagas', `${_.camelCase(featureName)}Sagas`)
  ));

  const featureSagas = utils.mapFeatureFile(featureName, 'redux/sagas.js');
  // create sagas.js entry file for the feature
  if (!fs.existsSync(featureSagas)) vio.save(featureSagas, '');
}

function afterRemoveFeature(featureName) {
  // Summary:
  //  Called after a feature is removed. Remove entry from rootSaga.js
  const rootSaga = utils.mapSrcFile('common/rootSaga.js');
  refactor.updateFile(rootSaga, ast => [].concat(
    refactor.removeImportBySource(ast, `../features/${_.kebabCase(featureName)}/redux/sagas`),
    refactor.removeFromArray(ast, 'featureSagas', `${_.camelCase(featureName)}Sagas`)
  ));
}

function afterMoveFeature(oldName, newName) {
  // Summary:
  //  Called after a feature is renamed. Rename entry in rootSaga.js
  const rootSaga = utils.mapSrcFile('common/rootSaga.js');
  refactor.updateFile(rootSaga, ast => [].concat(
    refactor.renameModuleSource(ast, `../features/${_.kebabCase(oldName)}/redux/sagas`, `../features/${_.kebabCase(newName)}/redux/sagas`),
    refactor.renameImportSpecifier(ast, `${_.camelCase(oldName)}Sagas`, `${_.camelCase(newName)}Sagas`)
  ));
}

module.exports = {
  afterAddFeature,
  afterRemoveFeature,
  afterMoveFeature,
};
```

### 使用插件

对于本地插件，除了创建它之外，您不需要做任何其他事情。如果存在冲突，处理元素类型的优先级最高。

对于公共插件，从npm安装。您需要在package.json的rekit部分注册它。

```
{
...,
"rekit": {
  "plugins": ["redux-saga", "apollo"], 
},
...,
}
```

这里你只需要在插件属性中定义公共插件，以便Rekit能够加载。本地插件将总是由Rekit加载。注意插件名称的顺序在它们接受相同的元素类型时很重要，而eariers则有更高的优先级。
虽然存在多个插件接受相同的元素类型的冲突，但从高到低的优先级是:本地插件<公共插件< Rekit默认行为。

> 注意:支持更多元素类型的插件现在只能通过命令行工具使用。

###　插件开发

对于大多数情况，一个插件只会基于一些模板创建一个`bolierplate`代码;在移动时重构代码，删除源文件。`Rekit-core`为促进插件开发提供了许多有用的APIs 。您可能只需要编写这些APIs 来满足您的需求。

### API参考

查看链接: [http://rekit.js.org/api/index.html](http://rekit.js.org/api/index.html)


## 路由

Rekit使用[React router](https://github.com/ReactTraining/react-router)作为路由解决方案。它几乎React web应用程序的标准方法。通过使用[React-router-redux](https://github.com/reactjs/react-router-redux) ，可以轻松使用 Redux store来同步路由状态。

即使对于简单的应用程序，路由也非常重要。就像传统的web应用程序需要不同页面的不同url一样，SPA也需要将不同的逻辑分组到不同的UI，这是Rekit的页面理念。

使用Rekit，页面通常是与URL路径关联的元素。每当通过Rekit创建页面时，它自动定义路由配置中的映射规则。

### 路由配置

由于Rekit使用面向功能的文件夹结构，因此路由配置也如此。这是在功能文件夹中定义的所有功能相关的路由配置。每个`feature`都有一个定义路由规则的`route.js`文件。下面是一个配置示例:

```
import {
  EditPage,
  ListPage,
  ViewPage,
} from './index';

export default {
  path: '',
  name: '',
  childRoutes: [
    { path: '', component: ListPage, name: 'Topic List', isIndex: true },
    { path: 'topic/add', component: EditPage, name: 'New Topic' },
    { path: 'topic/:topicId', component: ViewPage },
  ],
};
```

### 使用JavaScript定义路由规则

从上面的示例中，我们可以看到route config使用JavaScript。实际上，路由器支持[Json for configuration](https://github.com/ReactTraining/react-router/blob/master/docs/guides/RouteConfiguration.md)。Rekit使用了它，因此很容易将不同的路由配置分离成不同的` features`。下面是一个Json路由配置示例，来自`React router`官方文档:

```
const routes = {
  path: '/',
  component: App,
  indexRoute: { component: Dashboard },
  childRoutes: [
    { path: 'about', component: About },
    {
      component: Inbox,
      childRoutes: [{
        path: 'messages/:id', component: Message
      }]
    }
  ]
}
```

下面是Rekit使用的模式，以便在其 `features`中定义不同的路由配置:

```
import topicRoute from '../features/topic/route';
import commentRoute from '../features/comment/route';

const routes = [{
  path: '/rekit-example',
  component: App,
  childRoutes: [
    topicRoute,
    commentRoute,
    { path: '*', name: 'Page not found', component: PageNotFound },
  ],
}];
```

它展示了如何从 features `topic`和`comment`中导入和使用路径配置。

> 注意，您不需要维护`src/common/routeConfig.js`。在添加/删除一个feature时，Rekit将自动添加和删除规则。

###　使用isIndex属性代替indexRoute


不像用JSX的方式，用来`<IndexRoute ...>`标签来配置一个路由，JavaScript API的官方`indexRoute`配置是Rekit的一个难点，Rekit添加了isIndex属性支持。
一个路由规则如果设置`isIndex: true`，它将成为父索引路由
下面的代码是自动创建的home feature的路由配置:

```
import {
  DefaultPage,
  TestPage1,
  TestPage2,
} from './index';

export default {
  path: '',
  name: 'home',
  childRoutes: [
    { path: 'default-page', component: DefaultPage, isIndex: true },
    { path: 'test-page-1', component: TestPage1 },
    { path: 'test-page-2', component: TestPage2 },
  ],
};
```

然后，DefaultPage将会成为根路径的索引路由。


### name属性

您可能已经注意到，route配置规则有一个`name`属性。实际上它只被`SimpleNav`组件使用。它只是在dev time中显示路由配置的一个方便的组件。您已经在欢迎页面中看到了。对于一个实际的应用程序来说，它可能是无用的。路由配置API的所有其他用法都与官方的方式相同，您可以参考 [React-router官方文档](https://github.com/ReactTraining/react-router/blob/master/docs/guides/RouteConfiguration.md)。


## 样式

Rekit使用[Less](http://lesscss.org/)作为的css预处理器，因为我已经习惯了它很长时间。[Scss](http://sass-lang.com/guide)支持将成为候选。实际上，在我的选择中，两者之间并没有太大的差别，而且很容易切换。

在许多实践中，React component中会导入less文件，不同的是，Rekit建议只使用 `Less`自己来管理依赖项。这种方法有几个优点:


1. Css可以构建并生成separtely。
2. React component导入less不是标准的方法，仅是webpack加载器的特性。
3. 如果组件使用不止一次，webpack的`Css-loader`会为构建生成重复的Css代码。


Rekit的使用非常直观的，如下图所示:

<img src="http://rekit.js.org/images/styling.png" width="600" alt="Styling">

一般来说，一个Rekit应用程序的样式遵循以下几个规则:

1. 全局样式定义在`src/styles/global.less`，例如css for body,h1,h2…

2. 每个组件都有一个具有相同名称的样式文件，例如，组件`SimpleNav。js`有一个名为`SimpleNave.less`的样式文件。

3. 每个`feature`都有一个名为`style.less`文件。为页面和组件导入所必需的样式文件。文件中还定义了所有 feature 的共同样式。

4. `src/styles/index.less`是导入所有feature的`style.less`和`global.less`样式的入口文件。

对于其他场景，您可以随意使用您喜欢的方式。


## 测试

测试一个React + Redux应用是困难的，你需要知道很多关于 React测试的 `libs/tools`，了解它们的用法。 但Rekit会为你设置了一切，您只需要在自动生成的测试文件中编写测试代码，在 Rekit portal中运行测试，就可以浏览器中读取测试覆盖率报告。

下面是React测试工作的过程，您可以了解Rekit是如何设置测试过程的。

1. [Istanbul](https://github.com/gotwarlost/istanbul)测试覆盖报告的源代码。Istanbul本身不支持JSX，但有一个babel插件[babel-plugin-istanbul](https://github.com/istanbuljs/babel-plugin-istanbul)能够支持运行。

2. Webpack用[mocha-webpack](https://github.com/zinserjan/mocha-webpack)构建所有测试文件，自动找到测试文件进行运行测试。

3. [Mocha](https://github.com/mochajs/mocha)运行测试文件的构建版本。

4. [nyc](https://github.com/istanbuljs/nyc)生成测试覆盖率报告。

所有应用程序测试文件都保存在`test/app`目录中。文件夹结构与`src`文件夹中的源代码文件夹结构相同。

### Rekit如何生成测试文件

在创建新页面、组件或action时，Rekit 自动为它们创建测试用例。即使您不向生成的测试用例添加任何代码，它们也可以帮助您避免简单的错误，如:

1. 确保了一个组件是否能渲染成功是通过检查DOM根节点的存在和正确的css类来完成的。

2. 通过检查前后状态是否一致来确保reducer不变。

例如，当创建一个新页面时，它生成以下测试案例:

```
it('renders node with correct class name', () => {
  const pageProps = {
    home: {},
    actions: {},
  };
  const renderedComponent = shallow(
    <TestPage1 {...pageProps} />
  );

  expect(
    renderedComponent.find('.home-test-page-1').node
  ).to.exist;
});
```

当创建一个action时，它生成下面的reducer测试用例：

```
it('reducer should handle MY_ACTION', () => {
  const prevState = {};
  const state = reducer(
    prevState,
    { type: MY_ACTION }
  );
  expect(state).to.not.equal(prevState); // should be immutable
  expect(state).to.deep.equal(prevState); // TODO: replace this line with real case.
});
```

### 命令行工具测试

所有命令行工具也有保存在`test/cli`文件夹中的单元测试用例。不仅在Rekit项目中，而且它们也被复制到创建的应用程序中，这样您就可以根据测试用例自定义工具。

因为这些工具脚本是用纯NodeJs编写的，所以测试更简单:

1. Istanbul 的源代码。
2. `Mocha`运行测试用例。
3. nyc生成测试覆盖率报告。

**run_test.js**

您可能已经注意到，测试用例也需要使用webpack构建，然后用Mocha可以运行测试用例。因此，运行单个测试用例文件也需要构建。所以 `run_test.js`的创建是为了简化这个过程。您可以只通过`run_test.js`工具运行任何测试用例文件。它也被导出为`npm test`。所有的参数都传递给`run_test.js`脚本。用法如下:

    npm test  // run all tests under the `test/app` folder
    npm test app/features/home // run tests under the `home` feature
    npm test app/**/list.test.js // run all tests named list.test.js under app folder
    npm test cli // run tests for all command line tools
    npm test all // run tests for both app and cli

### 查看测试覆盖率报告

如果测试是由`app`, `cli` or `all`运行的，那么所有测试覆盖率报告都将涉及。

可以看到如下的不同覆盖率报告:

1. `all`: coverage/lcov-report/index.html
2. `app`: coverage/app/lcov-report/index.html
3. `cli`: coverage/cli/lcov-report/index.html


## 打包

Webpack总被认为是构建React应用程序的最困难的地方。幸运的是，Rekit为你配置了一切，下面有两个webpack配置文件对应不同的用法:

- `webpack.config.js`:是webpack用于开发的配置工厂，`dll`和`dist`打包。
- `webpack.test.config.js`:测试构建的配置。

### 使用webpack-dll-plugin提高构建性能

当一个React应用程序变大时，开发构建应用程序会很耗时。`webpack-dll-plugin`可以解决这个痛点。有一个很好的文章对其进行了介绍:[http://engineering.invisionapp.com/post/optimizing-webpack/](http://engineering.invisionapp.com/post/optimizing-webpack/)

基本思想是将诸如 React, Redux, React-router之类的公共libs构建为一个单独的dll包。这样，在每次更改代码时都不需要构建它们。通过此方法，可以显著减少构建时间。

Rekit在`tools/server.js`集成了打包过程，用`npm start`即可运行。脚本检查`dll`构建所需的所有的版本包。因此，当任何依赖版本发生更改时，它将自动构建一个新的`dll`。

## 代码质量检查

Rekit使用ESlint对代码质量进行检查。并将[airbnb javascript guide](https://github.com/airbnb/javascript)作为所有代码的基本规则。`.eslintrc`定义了一些细微的变化用于Rekit应用程序的不同部分。

因为似乎规则表明airbnb经常改变，对于一个新创建的Rekit应用程序，可能有一些eslint错误，您需要更新代码或添加自己的规则以清除这些错误。
