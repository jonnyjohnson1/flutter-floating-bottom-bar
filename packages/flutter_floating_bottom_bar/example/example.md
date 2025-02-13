Simple basic example using only required arguments - `child` and `body`.

```dart
    BottomBar(
          child: Padding(
            padding: const EdgeInsets.all(16.0),
            child: Text(
              "This is the floating widget",
              textAlign: TextAlign.center,
              style: TextStyle(color: Colors.white),
            ),
          ),
          body: (context, controller) =>
              InfiniteListPage(controller: controller, color: Colors.blue,
    )
```

Simple basic example that demonstrates all properties of the `BottomBar` widget, and how to use them.

```dart

class MyHomePage extends StatefulWidget {
  MyHomePage({Key? key, required this.title}) : super(key: key);
  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage>
    with SingleTickerProviderStateMixin {
  late int currentPage;
  late TabController tabController;
  final List<Color> colors = [
    Colors.yellow,
    Colors.red,
    Colors.green,
    Colors.blue,
    Colors.pink
  ];

  @override
  void initState() {
    currentPage = 0;
    tabController = TabController(length: 5, vsync: this);
    tabController.animation!.addListener(
      () {
        final value = tabController.animation!.value.round();
        if (value != currentPage && mounted) {
          changePage(value);
        }
      },
    );
    super.initState();
  }

  void changePage(int newPage) {
    setState(() {
      currentPage = newPage;
    });
  }

  @override
  void dispose() {
    tabController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final Color unselectedColor = colors[currentPage].computeLuminance() < 0.5
        ? Colors.black
        : Colors.white;
    return SafeArea(
      child: Scaffold(
        appBar: AppBar(
          title: Text(widget.title),
          backgroundColor: Colors.black,
        ),
        body: BottomBar(
          child: TabBar(
            indicatorPadding: const EdgeInsets.fromLTRB(6, 0, 6, 0),
            controller: tabController,
            indicator: UnderlineTabIndicator(
                borderSide: BorderSide(
                    color: currentPage == 0
                        ? colors[0]
                        : currentPage == 1
                            ? colors[1]
                            : currentPage == 2
                                ? colors[2]
                                : currentPage == 3
                                    ? colors[3]
                                    : currentPage == 4
                                        ? colors[4]
                                        : unselectedColor,
                    width: 4),
                insets: EdgeInsets.fromLTRB(16, 0, 16, 8)),
            tabs: [
              SizedBox(
                height: 55,
                width: 40,
                child: Center(
                    child: Icon(
                  Icons.home,
                  color: currentPage == 0 ? colors[0] : unselectedColor,
                )),
              ),
              SizedBox(
                height: 55,
                width: 40,
                child: Center(
                    child: Icon(
                  Icons.search,
                  color: currentPage == 1 ? colors[1] : unselectedColor,
                )),
              ),
              SizedBox(
                height: 55,
                width: 40,
                child: Center(
                    child: Icon(
                  Icons.add,
                  color: currentPage == 2 ? colors[2] : unselectedColor,
                )),
              ),
              SizedBox(
                height: 55,
                width: 40,
                child: Center(
                    child: Icon(
                  Icons.favorite,
                  color: currentPage == 3 ? colors[3] : unselectedColor,
                )),
              ),
              SizedBox(
                height: 55,
                width: 40,
                child: Center(
                    child: Icon(
                  Icons.settings,
                  color: currentPage == 4 ? colors[4] : unselectedColor,
                )),
              ),
            ],
          ),
          fit: StackFit.expand,
          icon: Center(
            child: IconButton(
              padding: EdgeInsets.zero,
              onPressed: null,
              icon: Icon(
                Icons.arrow_upward_rounded,
                color: unselectedColor,
              ),
            ),
          ),
          borderRadius: BorderRadius.circular(500),
          duration: Duration(seconds: 1),
          curve: Curves.decelerate,
          showIcon: true,
          width: MediaQuery.of(context).size.width * 0.8,
          barColor: colors[currentPage].computeLuminance() > 0.5
              ? Colors.black
              : Colors.white,
          start: 2,
          end: 0,
          bottom: 10,
          alignment: Alignment.bottomCenter,
          iconHeight: 35,
          iconWidth: 35,
          reverse: false,
          hideOnScroll: true,
          scrollOpposite: false,
          onBottomBarHidden: () {},
          onBottomBarShown: () {},
          body: (context, controller) => TabBarView(
            controller: tabController,
            dragStartBehavior: DragStartBehavior.down,
            physics: const BouncingScrollPhysics(),
            children: colors
                .map((e) => InfiniteListPage(controller: controller, color: e))
                .toList(),
          ),
        ),
      ),
    );
  }
}
```
### With Floating action button

Simple basic example with a floating action button to achieve this effect.

![image](https://user-images.githubusercontent.com/60510869/183573165-28e6b896-6559-4d86-897a-3bc8b0adb927.png)

```dart


class MyHomePage extends StatefulWidget {
  MyHomePage({Key? key, required this.title}) : super(key: key);
  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> with SingleTickerProviderStateMixin {
  late int currentPage;
  late TabController tabController;
  final List<Color> colors = [
    Colors.yellow,
    Colors.red,
    Colors.green,
    Colors.blue,
    Colors.pink,
  ];

  @override
  void initState() {
    currentPage = 0;
    tabController = TabController(length: 5, vsync: this);
    tabController.animation?.addListener(
      () {
        final value = tabController.animation!.value.round();
        if (value != currentPage && mounted) {
          changePage(value);
        }
      },
    );
    super.initState();
  }

  void changePage(int newPage) {
    setState(() {
      currentPage = newPage;
    });
  }

  @override
  void dispose() {
    tabController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final Color unselectedColor = colors[currentPage].computeLuminance() < 0.5 ? Colors.black : Colors.white;
    final Color unselectedColorReverse = colors[currentPage].computeLuminance() < 0.5 ? Colors.white : Colors.black;

    return SafeArea(
      child: Scaffold(
        appBar: AppBar(
          title: Text(widget.title),
          backgroundColor: Colors.black,
        ),
        body: BottomBar(
          clip: Clip.none,
          child: Stack(
            alignment: Alignment.center,
            clipBehavior: Clip.none,
            children: [
              TabBar(
                indicatorPadding: const EdgeInsets.fromLTRB(6, 0, 6, 0),
                controller: tabController,
                indicator: UnderlineTabIndicator(
                    borderSide: BorderSide(
                      color: currentPage <= 4 ? colors[currentPage] : unselectedColor,
                      width: 4,
                    ),
                    insets: EdgeInsets.fromLTRB(16, 0, 16, 8)),
                tabs: [
                  SizedBox(
                    height: 55,
                    width: 40,
                    child: Center(
                        child: Icon(
                      Icons.home,
                      color: currentPage == 0 ? colors[0] : unselectedColor,
                    )),
                  ),
                  SizedBox(
                    height: 55,
                    width: 40,
                    child: Center(
                      child: Icon(
                        Icons.search,
                        color: currentPage == 1 ? colors[1] : unselectedColor,
                      ),
                    ),
                  ),
                  SizedBox(
                    height: 55,
                    width: 40,
                    child: Center(
                      child: Icon(
                        Icons.add,
                        color: currentPage == 2 ? colors[2] : unselectedColorReverse,
                      ),
                    ),
                  ),
                  SizedBox(
                    height: 55,
                    width: 40,
                    child: Center(
                      child: Icon(
                        Icons.favorite,
                        color: currentPage == 3 ? colors[3] : unselectedColor,
                      ),
                    ),
                  ),
                  SizedBox(
                    height: 55,
                    width: 40,
                    child: Center(
                      child: Icon(
                        Icons.settings,
                        color: currentPage == 4 ? colors[4] : unselectedColor,
                      ),
                    ),
                  ),
                ],
              ),
              Positioned(
                top: -25,
                child: FloatingActionButton(
                  onPressed: () {},
                  child: Icon(Icons.add),
                ),
              )
            ],
          ),
          fit: StackFit.expand,
          icon: (width, height) => Center(
            child: IconButton(
              padding: EdgeInsets.zero,
              onPressed: null,
              icon: Icon(
                Icons.arrow_upward_rounded,
                color: unselectedColor,
                size: width,
              ),
            ),
          ),
          borderRadius: BorderRadius.circular(500),
          duration: Duration(milliseconds: 500),
          curve: Curves.decelerate,
          showIcon: true,
          width: MediaQuery.of(context).size.width * 0.8,
          barColor: colors[currentPage].computeLuminance() > 0.5 ? Colors.black : Colors.white,
          start: 2,
          end: 0,
          bottom: 10,
          alignment: Alignment.bottomCenter,
          iconHeight: 30,
          iconWidth: 30,
          reverse: false,
          hideOnScroll: true,
          scrollOpposite: false,
          onBottomBarHidden: () {},
          onBottomBarShown: () {},
          body: (context, controller) => TabBarView(
            controller: tabController,
            dragStartBehavior: DragStartBehavior.down,
            physics: const BouncingScrollPhysics(),
            children: colors
                .map(
                  (e) => InfiniteListPage(
                    key: ValueKey('infinite_list_key#${e.toString()}'),
                    controller: controller,
                    color: e,
                  ),
                )
                .toList(),
          ),
        ),
      ),
    );
  }
}

```

### Floating Bottom Search Bar

A simple Material search bar in the bottom of your app, which hides on scroll down.

```dart

class MyHomePage extends StatelessWidget {
  MyHomePage({Key? key, required this.title}) : super(key: key);
  final String title;

  @override
  Widget build(BuildContext context) {
    return SafeArea(
      child: Scaffold(
        appBar: AppBar(
          title: Text(title),
          backgroundColor: Colors.black,
        ),
        body: BottomBar(
          child: Padding(
            padding: const EdgeInsets.all(8.0),
            child: Card(
              elevation: 10,
              child: Container(
                height: 46,
                child: TextField(
                  style: Theme.of(context).textTheme.subtitle2,
                  maxLines: 1,
                  minLines: 1,
                  textCapitalization: TextCapitalization.sentences,
                  cursorColor: Theme.of(context).accentColor,
                  decoration: InputDecoration(
                    suffixIcon: Icon(Icons.search),
                    contentPadding: const EdgeInsets.all(16),
                    hintStyle: Theme.of(context).textTheme.subtitle2,
                    hintText: 'Search...',
                    border: InputBorder.none,
                    labelStyle: Theme.of(context).textTheme.subtitle2,
                  ),
                ),
              ),
            ),
          ),
          fit: StackFit.expand,
          borderRadius: BorderRadius.circular(12),
          duration: Duration(milliseconds: 300),
          curve: Curves.decelerate,
          showIcon: false,
          width: MediaQuery.of(context).size.width - 32,
          barColor: Colors.transparent,
          start: 2,
          end: 0,
          bottom: 10,
          alignment: Alignment.bottomCenter,
          body: (context, controller) => InfiniteListPage(
              controller: controller, color: Colors.blueAccent),
        ),
      ),
    );
  }
}
```

### Floating Search Top Bar

A simple Material search bar in the top of your app, which hides on scroll down.

```dart
# Simply change these values in the bottom search bar to convert it into top search bar
          start: 0,
          end: 2,
          bottom: MediaQuery.of(context).size.height,
```
