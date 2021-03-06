---
layout: default
---


### Quick tutorial part 2 - Setting up the delegates

From part one of your tutorial you calendar looked like this.

<img width="311" alt="completecal" src="https://cloud.githubusercontent.com/assets/2439146/19029087/ad30b7ac-88f7-11e6-9ae5-b9d0ac5c837b.png">

It's time to add some more functionality.

### Let's add a yellow-colored selected view to the calendar.

We want to add a yellow-colored `selectedView` to the calendar that will appear whenever you tap on a dateCell.

Head to your `CellView.xib` file and drag a new `UIView` unto the canvas. Make sure the newly-dragged view is **above** the Day Label view. Why? Because we want the selectedView to display behind the DayLabel.

<img width="424" src="https://cloud.githubusercontent.com/assets/2439146/19415251/bafe16ea-931f-11e6-9fd7-6837fc932cc4.png">

I have set constraints to have both the `width` and the `height` of the view to `50.00`. I have also set the view to be centered both horizontally and vertically in the cell.

Go to your `CellView.Swift` file and add the following variable so that you can connect it to the new dragged view.

**Paste the following code to your `CellView.swift` file**

```swift
@IBOutlet var selectedView: UIView!
```

Head back to the CellView.xib file and connect the new outlet you have created.

### Now let's write some code.
___

In your `ViewController.swift` file, paste the following code in your extension.

```swift
func calendar(_ calendar: JTAppleCalendarView, didSelectDate date: Date, cell: JTAppleDayCellView?, cellState: CellState) {
    let myCustomCell = cell as! CellView
    
    // Let's make the view have rounded corners. Set corner radius to 25
    myCustomCell.selectedView.layer.cornerRadius =  25
    
    if cellState.isSelected {
        myCustomCell.selectedView.isHidden = false
    }
}
```

This code handles the selection. Now **paste the following code to handle the deselection**

```swift
func calendar(_ calendar: JTAppleCalendarView, didDeselectDate date: Date, cell: JTAppleDayCellView?, cellState: CellState) {
    let myCustomCell = cell as! CellView
    myCustomCell.selectedView.isHidden = true
}
```

If you run the app, selection will now work. But if you scroll, you'll see selectedViews being repeated randomly across the calendar.

Why is this?

**Important**: JTAppleCalendar is a `UICollectionView` subclass. Therefore, cells are being re-used when the calendar is scrolled. 

Therefore, you will also need code in your **willDisplayCell** delegate function to determine when to show and hide the `selectedView` once your calendar is scrolled.

To avoid code repetition, let's create a function to handle the showing/hiding of the selectedView.


**Paste the following code in your viewController extension**:

```swift
func handleCellSelection(view: JTAppleDayCellView?, cellState: CellState) {
    guard let myCustomCell = view as? CellView  else {
        return
    }
    if cellState.isSelected {
        myCustomCell.selectedView.layer.cornerRadius =  25
        myCustomCell.selectedView.isHidden = false
    } else {
        myCustomCell.selectedView.isHidden = true
    }
}
```

Finally, these 3 function's code should be modified to the following:

```swift
func calendar(_ calendar: JTAppleCalendarView, willDisplayCell cell: JTAppleDayCellView, date: Date, cellState: CellState) {
    let myCustomCell = cell as! CellView
    
    // Setup Cell text
    myCustomCell.dayLabel.text = cellState.text
    
    // Setup text color
    if cellState.dateBelongsTo == .thisMonth {
        myCustomCell.dayLabel.textColor = UIColor(colorWithHexValue: 0xECEAED)
    } else {
        myCustomCell.dayLabel.textColor = UIColor(colorWithHexValue: 0x574865)
    }
    
    handleCellSelection(view: cell, cellState: cellState)
}

func calendar(_ calendar: JTAppleCalendarView, didSelectDate date: Date, cell: JTAppleDayCellView?, cellState: CellState) {
    handleCellSelection(view: cell, cellState: cellState)
}

func calendar(_ calendar: JTAppleCalendarView, didDeselectDate date: Date, cell: JTAppleDayCellView?, cellState: CellState) {
    handleCellSelection(view: cell, cellState: cellState)
}
```

Selection should now be complete.

