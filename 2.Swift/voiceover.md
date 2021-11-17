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
```

### Basic operations
1. **Unlock iPhone**: Wake iPhone and glance at it, then drag up from the bottom edge of the screen until you feel a vibration or hear two rising tones.  
2. **Read out**: Single-tap will read the label out.
3. **Switch Element**: Swipe right to select next element, Swipe left to select the previous element.
4. **Click Element**: After you select an element(there should be a focus cursor area surrounds the element), Double-tap anywhere to click it(Open apps/click buttons).
5. **Scroll Page/Screen**: Swipe right with three fingers to show next page(works for both horizontal and vertical directions); Swipe left with three fingers to show perview page.
6. **Swift to another app:1**: Swipe right or left with four fingers to cycle through the open apps(You should in one of those apps first, if you're at home screen, nonthing will happen).
7. **Swift to another app:2**: 1) Drag one finger up from the bottom edge of the screen until you feel the second vibration or hear three tones, then lift your finger. 2) Swipe left or right until the app you want is selected. 3) Double-tap to open the app. 

Every time you want to interact with an element, you should focus it first, by single-tap it. Once it's focused, you can double tap anywhere to click the element, or three fingers to scroll, otherwise you may wondering why the screen not response to you.

here is a guide from Apple: [Operate iPhone using VoiceOver gestures](https://support.apple.com/en-hk/guide/iphone/iph3e2e2329/ios)

## Add VoiceOver in your iOS application.
There're 3 things you can add:
#### Label

> The label attribute identifies the user interface element.
1. Very briefly describes the element.
2. Dont' include the type of the control or view(It's trait's work).
3. Begins with a capital word.
4. Does not end with a period.
5. Is localized.

#### Hint
> The hint attribute describes the results of performing an action on a control or a view.
1. Very briefly describes the results.
2. Begins with a verb and omits the subject.
3. Begins with a capitalized word and ends with a period.
4. Does not include the name of the action or gesture.
5. Does not include the name of the control or view.
6. Does not include the type of the control or view.
7. Is localized.

#### Trait
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

### How to focus on a specified element

#### Solution1: [iOS change accessibility focus.](https://stackoverflow.com/questions/7529464/ios-change-accessibility-focus)
```swift
let titleLabel = UILabel()

override func viewDidAppear(_ animated: Bool) {
    super.viewDidAppear(animated)
    UIAccessibility.post(notification: .screenChanged, argument: titleLabel)
}

// or Using:
UIAccessibility.post(notification: .layoutChanged, argument: object)
```
#### Solution: [Change order of read items with VoiceOver](https://stackoverflow.com/questions/13279498/change-order-of-read-items-with-voiceover/13407513#13407513)
```c
// By:
-(NSInteger)accessibilityElementCount
-(id)accessibilityElementAtIndex:
-(NSInteger)indexOfAccessibilityElement:

// Or by:
view.accessibilityElements = [view1, view2, view3]
```

## Links
* [Accessibility Programming Guide for iOS](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/iPhoneAccessibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008785-CH1-SW1)
* [Supporting VoiceOver in Your App](https://developer.apple.com/documentation/accessibility/supporting_voiceover_in_your_app/)
* [带你认识——iOS Accessibility](https://www.jianshu.com/p/0991a4f0bc0c)
