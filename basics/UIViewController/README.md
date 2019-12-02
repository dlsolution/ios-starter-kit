# UIViewController

## How to present modally

<img src="https://github.com/jrasmusson/ios-starter-kit/blob/master/basics/UIViewController/images/modal.png" alt="drawing" width="400"/>

- To present modally you want to use the `present` method.
- Don't `present` in `viewDidLoad()`. View hierarchy has not been loaded yet.
- Instead do it in `viewDidAppear()`. View hierarchy has been established.

```swift
class ContainerViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .red
    }
}

class LaunchViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        view.backgroundColor = .blue
    }

    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)

        let containerViewController = ContainerViewController() 

        // modal
        present(containerViewController, animated: true)
    }

}
```

> Note: This will present the viewController modally (as of iOS13). To present as full screen take over you need to `addChild`.


## How to present full screen

<img src="https://github.com/jrasmusson/ios-starter-kit/blob/master/basics/UIViewController/images/fullscreen.png" alt="drawing" width="400"/>

```swift
override func viewDidAppear(_ animated: Bool) {
    super.viewDidAppear(animated)

    let containerViewController = ContainerViewController()

    // full screen
    // The x3 things we need to do when presented a child view controller within a parent
    view.addSubview(containerViewController.view)
    addChild(containerViewController)
    containerViewController.didMove(toParent: self)
}
```

Or use this extension

```swift
extension UIViewController {
    func add(_ child: UIViewController) {
        addChild(child)
        view.addSubview(child.view)
        child.didMove(toParent: self)
    }

    func remove() {
        // Just to be safe, we check that this view controller
        // is actually added to a parent before removing it.
        guard parent != nil else {
            return
        }

        willMove(toParent: nil)
        view.removeFromSuperview()
        removeFromParent()
    }
}
```


### Links that help

- [Sundell How to add child viewController](https://www.swiftbysundell.com/articles/using-child-view-controllers-as-plugins-in-swift/)