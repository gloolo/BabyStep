# VoiceOver: Start from scratch.
VoiceOver is designed to help the users who are bind or have low vision, with the auditory feedback, users can experience without having to see the screen.


## VoiceOver guide for iPhone with FaceID
> As a developer, you should know the basic operations, then you will learn how far you could help those people and be aware of the limitations.

### Turn on/off VoiceOver?
1. Using path.
```
Settings > Accessibility > VoiceOver > VoiceOver On/Off
```
2. Alterlately, you can turn on/off with Siri, with those command.
```
Hi Siri, Open VoiceOver
Hi Siri, Close VoiceOver

or 使用中文
Hey Siri, 开启旁白
Hey Siri, 关闭旁白
```

### Basic operations
1. **Unlock iPhone**: Wake iPhone and glance at it, then drag up from the bottom edge of the screen until you feel a vibration or hear two rising tones.  
2. **Read out**: Single-tap will read the label out.
3. **Switch Element**: Swipe right to select next element, Swipe left to select the previous element.
4. **Click Element**: After you select an element(there should be a focus cursor area surrounds the element), Double-tap anywhere to click it(Open apps/click buttons).
5. **Scroll Page/Screen**: Swipe right with three fingers to show next page(works for both horizontal and vertical directions); Swipe left with three fingers to show perview page.
6. **Swift to another app:1**: Swipe right or left with four fingers to cycle through the open apps(You should in one of those apps first, if you're at home screen, nonthing will happen).
7. **Swift to another app:2**: 1) Drag one finger up from the bottom edge of the screen until you feel the second vibration or hear three tones, then lift your finger. 2) Swipe left or right until the app you want is selected. 3) Double-tap to open the app. 
8. **Change Slider value**: One-finger swipe up and down

Every time you want to interact with an element, you should focus it first, by single-tap it. Once it's focused, you can double tap anywhere to click the element, or three fingers to scroll, otherwise you may wondering why the screen not response to you.

Here is a guide from Apple: [Operate iPhone using VoiceOver gestures](https://support.apple.com/en-hk/guide/iphone/iph3e2e2329/ios)

## Add VoiceOver in your iOS application.
Normally, VoiceOver will speak out 3 kind of text:
#### 1. Label
> The label attribute identifies the user interface element.

Basic rules:
1. Very briefly describes the element.
2. Dont' include the type of the control or view(It's trait's work).
3. Begins with a capital word.
4. Does not end with a period.
5. Is localized.

#### 2. Hint
> The hint attribute describes the results of performing an action on a control or a view.

Basic rules:
1. Very briefly describes the results.
2. Begins with a verb and omits the subject.
3. Begins with a capitalized word and ends with a period.
4. Does not include the name of the action or gesture.
5. Does not include the name of the control or view.
6. Does not include the type of the control or view.
7. Is localized.

#### 3. Trait
> The traits attribute contains one or more individual traits that, taken together, describe the behavior of an accessible user interface element.

##### Common traits
* **Button**
* **Link**
* **Search Field**
* **Keyboard Key**
* Static Text
* Image
* Plays Sound
* Selected
* Summary Element
* Updates Frequently
* Not Enabled
* None

### Experimennts
#### Normal Views
```swift
//
//  ProvideInterface.swift
//  VoiceOverDemo
//
//  Created by qiang xu on 2021/11/18.
//

import Foundation
import UIKit

protocol ViewProviding {
    func contentView() -> UIView
}

// Read out "Normal Button"
class ButtonProvider: ViewProviding {
    func contentView() -> UIView {
        let btn = UIButton(type: .custom)
        btn.setTitle("Normal", for: .normal)
        btn.setTitle("HighLight", for: .highlighted)
        btn.addTarget(self, action: #selector(btnTapped(_:)), for: .touchUpInside)
        btn.setTitleColor(UIColor.red, for: .normal)
        return btn
    }
    
    @objc func btnTapped(_ sender: UIButton) {
        DispatchQueue.main.async {
            UIAccessibility.post(notification: .announcement, argument: "Clicked")
        }
    }
}

// Read out "Set accessibilityLabel Label"
struct LabelProvider: ViewProviding {
    func contentView() -> UIView {
        let label = UILabel()
        label.text = "Set Text"
        label.accessibilityLabel = "Set accessibilityLabel"
        
        return label
    }
}

// Read out
// TextField has accessibilityLabel
// TextField has texts
// TextField
// I'm accessibility hint
// Double Tap to edit
struct TextFieldProvider: ViewProviding {
    func contentView() -> UIView {
        let textField = UITextField()
        textField.text = "TextField has texts"
        textField.placeholder = "I'm placeholder"
        textField.accessibilityLabel = "TextField has accessibilityLabel"
        textField.accessibilityHint = "I'm accessibility hint"
        return textField
    }
}

// Read out
// TextView has accessibilityLabel
// I'm text view
// TextView
// I'm accessibility hint
// Use Rout....
class TextViewProvider: ViewProviding {
    
    func contentView() -> UIView {
        let textView = UITextView()
        textView.translatesAutoresizingMaskIntoConstraints = false
        textView.heightAnchor.constraint(equalToConstant: 80).isActive = true
        textView.backgroundColor = UIColor.purple
        textView.text = "I'm text view"
        textView.accessibilityLabel = "TextView has accessibilityLabel"
        textView.accessibilityHint = "I'm accessibility hint"
        textView.isScrollEnabled = false
        return textView
    }
}

// Read out
// Switch Button Off
// Double tap to toggle setting
struct SwitchProvider: ViewProviding {
    func contentView() -> UIView {
        UISwitch()
    }
}

// 45 percent
// ajustable
// swipe up/down with one finger to ajust value
class SliderProvider: ViewProviding {
    
    func contentView() -> UIView {
        let view = UISlider()
        view.minimumValue = 1
        view.maximumValue = 100
        view.value = 50
        view.translatesAutoresizingMaskIntoConstraints = true
        view.widthAnchor.constraint(equalToConstant: 100).isActive = true
        view.addTarget(self, action: #selector(slided(_:)), for: .valueChanged)
        return view
    }
    
    @objc func slided(_ slider: UISlider) {
        print("changed:\(slider.value)")
    }
}

// label1 label2 off
// tap twice to change the switch's control status
class GroupContainerViewProvider: ViewProviding {
    func contentView() -> UIView {
        return GroupContainerView()
    }
}

// label1 label2 Button label3
class ReorderContainerViewProvider: ViewProviding {
    func contentView() -> UIView {
        return ReorderContainer()
    }
}

// label1 label2 lable3 button
class BeforeReorderContainerViewProvider: ViewProviding {
    func contentView() -> UIView {
        return BeforeReorderContainer()
    }
}

class GroupContainerView: UIView {
    
    var label1: UILabel?
    var label2: UILabel?
    var toggle: UISwitch?
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        setupSubviews()
        
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    private func setupSubviews() {
        let label = UILabel()
        label.text = "label1"
        addSubview(label)
        
        let label2 = UILabel()
        label2.text = "label2"
        addSubview(label2)
        
        let aSwitch = UISwitch()
        addSubview(aSwitch)
        
        label.snp.makeConstraints { make in
            make.leading.centerY.equalTo(self)
        }
        
        label2.snp.makeConstraints { make in
            make.centerY.equalTo(label)
            make.leading.equalTo(label.snp.trailing)
        }
        isAccessibilityElement = true
        aSwitch.snp.makeConstraints { make in
            make.leading.equalTo(label2.snp.trailing)
        }
        accessibilityElements = [label, label2, aSwitch]
        accessibilityHint = "tap twice to change the switch control status."
        
        self.label1 = label
        self.label2 = label2
        self.toggle = aSwitch
        
        updateAccessibilityLabel()
    }
    
    override var intrinsicContentSize: CGSize {
        return CGSize(width: (label1?.intrinsicContentSize.width ?? 0.0) +  (label2?.intrinsicContentSize.width ?? 0.0) + (toggle?.intrinsicContentSize.width ?? 0.0), height: 48)
    }
    
    override func accessibilityActivate() -> Bool {
        toggle?.setOn(!(toggle?.isOn ?? false), animated: true)
        updateAccessibilityLabel()
        return true
    }
    
    private func updateAccessibilityLabel() {
        let content = (label1?.accessibilityLabel ?? "") + (label2?.accessibilityLabel ?? "") + (toggle?.isOn == true ? "on" : "off")
        print(content)
        accessibilityLabel = content
    }
}

class BeforeReorderContainer: UIView {
    
    var label1: UILabel?
    var label2: UILabel?
    var label3: UILabel?
    var button: UIButton?
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        setupSubviews()
        
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    private func setupSubviews() {
        let label = UILabel()
        label.text = "label1"
        addSubview(label)
        
        let label2 = UILabel()
        label2.text = "label2"
        addSubview(label2)
        
        let label3 = UILabel()
        label3.text = "label3"
        addSubview(label3)
        
        let aButton = UIButton(type: .custom)
        aButton.setTitleColor(.red, for: .normal)
        aButton.setTitle("Button", for: .normal)
        addSubview(aButton)
        
        label.snp.makeConstraints { make in
            make.leading.centerY.equalTo(self)
        }
        
        label2.snp.makeConstraints { make in
            make.centerY.equalTo(label)
            make.leading.equalTo(label.snp.trailing)
        }
        
        label3.snp.makeConstraints { make in
            make.top.equalTo(label.snp.bottom)
            make.leading.equalTo(label.snp.leading)
            make.trailing.equalTo(label2.snp.trailing)
            make.bottom.equalTo(self)
        }
        
        aButton.snp.makeConstraints { make in
            make.top.equalTo(label)
            make.leading.equalTo(label2.snp.trailing)
            make.trailing.equalTo(self)
        }

        self.label1 = label
        self.label2 = label2
        self.label3 = label3
        self.button = aButton
    }
    
    override var intrinsicContentSize: CGSize {
        let width: CGFloat = (label1?.intrinsicContentSize.width ?? 0.0) +  (label2?.intrinsicContentSize.width ?? 0.0) + (button?.intrinsicContentSize.width ?? 0.0)
        let height: CGFloat = (label1?.intrinsicContentSize.height ?? 0.0) + (label3?.intrinsicContentSize.height ?? 0.0)
        return CGSize(width: width, height: height)
    }
}

class ReorderContainer: UIView {
    
    var label1: UILabel?
    var label2: UILabel?
    var label3: UILabel?
    var button: UIButton?
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        setupSubviews()
        
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    private func setupSubviews() {
        let label = UILabel()
        label.text = "label1"
        addSubview(label)
        
        let label2 = UILabel()
        label2.text = "label2"
        addSubview(label2)
        
        let label3 = UILabel()
        label3.text = "reorder label3"
        addSubview(label3)
        
        let aButton = UIButton(type: .custom)
        aButton.setTitleColor(.red, for: .normal)
        aButton.setTitle("Button", for: .normal)
        addSubview(aButton)
        
        label.snp.makeConstraints { make in
            make.leading.centerY.equalTo(self)
        }
        
        label2.snp.makeConstraints { make in
            make.centerY.equalTo(label)
            make.leading.equalTo(label.snp.trailing)
        }
        
        label3.snp.makeConstraints { make in
            make.top.equalTo(label.snp.bottom)
            make.leading.equalTo(label.snp.leading)
            make.trailing.equalTo(label2.snp.trailing)
            make.bottom.equalTo(self)
        }
        
        aButton.snp.makeConstraints { make in
            make.top.equalTo(label)
            make.leading.equalTo(label2.snp.trailing)
            make.trailing.equalTo(self)
        }

        self.label1 = label
        self.label2 = label2
        self.label3 = label3
        self.button = aButton
        
        shouldGroupAccessibilityChildren = true
        isAccessibilityElement = false
        accessibilityElements = [label, label2, label3, aButton]
    }
    
    override var intrinsicContentSize: CGSize {
        let width: CGFloat = (label1?.intrinsicContentSize.width ?? 0.0) +  (label2?.intrinsicContentSize.width ?? 0.0) + (button?.intrinsicContentSize.width ?? 0.0)
        let height: CGFloat = (label1?.intrinsicContentSize.height ?? 0.0) + (label3?.intrinsicContentSize.height ?? 0.0)
        return CGSize(width: width, height: height)
    }
}
```

#### If a UILabel is setted `accessibilityLabel` different with label value, what will happen.  
Will read out `accessibilityLabel` instead of label value.

#### What will happen if a textField is configured like below.
```
textField.text = "TextField has texts"
textField.placeholder = "I'm placeholder"
textField.accessibilityLabel = "TextField has accessibilityLabel"
textField.accessibilityHint = "I'm accessibility hint"
```

The order of vocalization is always as follows: label, value, trait and hint.
It will read out orderly like this:
1. TextField has accessibilityLabel
2. TextField has texts
3. TextField
4. I'm accessibility hint, Double tap to edit.

If you remove the `text` parameter, then it will read out the placeholder if it exists. TextView works similarly, except it doesn't have the placeholder property. 

#### About push notificaitons
```swift
class NotificationsController: UIViewController {

    @IBOutlet weak var button: UIButton!

    // Will Focus again on back button
    @IBAction func pushScreenWithNil(_ sender: Any) {
        toggleButtonTitle()
        UIAccessibility.post(notification: .screenChanged, argument: nil)
    }
    
    // Will Focus on last button
    @IBAction func pushScreenNotification(_ sender: Any) {
        toggleButtonTitle()
        UIAccessibility.post(notification: .screenChanged, argument: button)
    }
    
    // focus cursor won't change
    @IBAction func pushLayoutNotificationWithNil(_ sender: Any) {
        toggleButtonTitle()
        UIAccessibility.post(notification: .layoutChanged, argument: nil)
    }
    
    // focus on last button
    @IBAction func pushLayoutNotification(_ sender: Any) {
        toggleButtonTitle()
        UIAccessibility.post(notification: .layoutChanged, argument: button)
    }
    
    // read "Hello announcement" out
    @IBAction func pushAnnouncementNotification(_ sender: Any) {
        UIAccessibility.post(notification: .announcement, argument: "Hello announcement")
    }
    
    // Nothing will happen
    @IBAction func pushInGlobalQueue(_ sender: Any) {
        DispatchQueue.global().async {
            self.pushAnnouncementNotification(sender)
        }
    }
    
    // Will read the "Third" only
    @IBAction func pushMutliNotification(_ sender: Any) {
        UIAccessibility.post(notification: .announcement, argument: "First")
        UIAccessibility.post(notification: .announcement, argument: "Second")
        UIAccessibility.post(notification: .announcement, argument: "Third")
        
    }
    
    private func toggleButtonTitle() {
        let current = button.currentTitle
        button.setTitle(current == "Butter" ? "Fly" : "Butter", for: .normal)
    }
}
```


## Links
* [Demo Link](https://github.com/gloolo/VoiceOverDemo)
* [Accessibility Programming Guide for iOS](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/iPhoneAccessibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008785-CH1-SW1)
* [Supporting VoiceOver in Your App](https://developer.apple.com/documentation/accessibility/supporting_voiceover_in_your_app/)
* [iOS developer guide 👍](https://a11y-guidelines.orange.com/en/mobile/ios/development/)
* [带你认识——iOS Accessibility](https://www.jianshu.com/p/0991a4f0bc0c)
* [iOS change accessibility focus.](https://stackoverflow.com/questions/7529464/ios-change-accessibility-focus)
* [Change order of read items with VoiceOver](https://stackoverflow.com/questions/13279498/change-order-of-read-items-with-voiceover/13407513#13407513)
