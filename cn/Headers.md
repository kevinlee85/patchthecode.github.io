---
layout: default
---

### What kind of header do you want to implement? There are 2 types.

## 1. Outside headers
___

You can implement headers `outside` of the calendar. These headers will not scroll with calendar-view and have nothing to do with the calendar as they are created by yourself.

<img width="250" src="https://cloud.githubusercontent.com/assets/2439146/19091493/bb69bf4e-8a37-11e6-9af7-4790c3c45451.png">

The following is an example of such a header created with 7 labels in a stack view.


<img src="https://cloud.githubusercontent.com/assets/2439146/19091368/324df7b6-8a37-11e6-8946-52ba7b1276b0.gif" height="250" width="250">

## 2. Inside headers
___

Inside headers are generated by the calendar. These headers scroll with the calendar. You can implement multiple headers, or single headers.

<img src="https://cloud.githubusercontent.com/assets/2439146/19060490/904238a4-899d-11e6-8c11-36a9fa0991cd.gif" height="250" width="250">

**(A) Implementing single headers**

 * First create the header.
 * (XCode 8) File -> New -> View

 <img width="550" alt="screen shot 2016-10-04 at 2 52 19 pm" src="https://cloud.githubusercontent.com/assets/2439146/19093972/6f6eeda2-8a42-11e6-8f78-7f8f45dbfe7b.png">
 
 Let's save this as `PinkSectionHeaderView.xib`. Your view may look a bit huge, so click on theView -> then attributes inspector, and change the size to `freeform`. Then adjust the size to your liking.  Also, add a label in the center of the view and center it with autolayout constraints. I made the back color pink.
 
 <img width="650" src="https://cloud.githubusercontent.com/assets/2439146/19095797/ebff1324-8a4c-11e6-8f8a-252b08abcf3b.png">


* With your header designs done, create a new class for your xib. I'll call mine `PinkSectionHeaderView.swift`. Paste the following code in your class:

```swift
   import JTAppleCalendar
   class PinkSectionHeaderView: JTAppleHeaderView {
       @IBOutlet var title: UILabel!
   }
```

* Head back to your header xib file. Click on the root view, then click on the identity inspector. Change the name of the class to `PinkSectionHeaderView`. Then hook up the IBOutlet for your UILabel.

   <img width="250" alt="screen shot 2016-10-04 at 3 53 50 pm" src="https://cloud.githubusercontent.com/assets/2439146/19095599/7c95f102-8a4b-11e6-9410-2c78d7187214.png">

* In your `viewDidLoad` function on your ViewController, register your header by pasting the following.

```swift
calendarView.registerHeaderView(xibFileNames: ["PinkSectionHeaderView"]).
```

* Implement the following two delegate functions in your ViewController.

```swift
   // This sets the height of your header
   func calendar(_ calendar: JTAppleCalendarView, sectionHeaderSizeFor range: (start: Date, end: Date), belongingTo month: Int) -> CGSize {
      return CGSize(width: 200, height: 50)
   }
   // This setups the display of your header
   func calendar(_ calendar: JTAppleCalendarView, willDisplaySectionHeader header: JTAppleHeaderView, range: (start: Date, end: Date), identifier: String) {
      let headerCell = (header as? PinkSectionHeaderView)
      headerCell?.title.text = "Hello Header"
   }
``` 
   Completed.

**(B) Implementing multiple headers**

* The steps are exactly the same as implementing a single header shown above. The only difference is that you need to register the headers.

```swift
// Put this code in your viewDidLoad function
calendarView.registerHeaderView(xibFileNames: ["PinkSectionHeaderView", "WhiteSectionHeaderView"])

// Also add this delegate function.
// This tells the calendar which headerView you want displayed for a particular date.  
// The names should be the same as what was registered.
func calendar(_ calendar: JTAppleCalendarView, sectionHeaderIdentifierFor range: (start: Date, end: Date), belongingTo month: Int) -> String {
    if month % 2 > 0 {
       return "WhiteSectionHeaderView"
    }
    return "PinkSectionHeaderView"
}
```