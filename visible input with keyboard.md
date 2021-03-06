# UIKit Cheats

## Keep input visible when keyboard is open 

```Swift
// Received notification that a keyboard will appear
 func keyboardNotification(_ notification: Notification){
        if let userInfo = (notification as NSNotification).userInfo {
            
            //Size of keyboard
            guard let endFrame = (userInfo[UIKeyboardFrameEndUserInfoKey] as? NSValue)?.cgRectValue else {
                return
            }
            
            var hasPhysicalKeyboard:Bool = false
            
            let keyboard = self.view.convert(endFrame, from: self.view.window)
            
            let heightOfCurrentView = self.view.frame.size.height
            
            //Check if keyboard is out of bounds, if yes, then its a physical keyboard
            
            if (keyboard.origin.y + keyboard.size.height) > (heightOfCurrentView + self.navigationBarHeight + self.tabBarHeight) {
                hasPhysicalKeyboard = true
            }
            
            if !hasPhysicalKeyboard {
                
                // Move the scroll view (in tableViewGroup) if the textField (activeField) is hidden by the keyboard
                let contentInsets = UIEdgeInsetsMake(0.0, 0.0, endFrame.size.height, 0.0)
                tableViewGrouped.contentInset = contentInsets
                tableViewGrouped.scrollIndicatorInsets = contentInsets
                var rectCurrent = self.view.frame
                let currentHeight = rectCurrent.size.height
                rectCurrent.size.height  = (currentHeight - endFrame.size.height - THKUIReference.toolbarHeight)
                
                // Check if the textfield is hidden, if not make this rectangle visible
                if (!rectCurrent.contains(THKCellEditProductPrice.activeField.frame.origin)) {
                    self.tableViewGrouped.scrollRectToVisible(THKCellEditProductPrice.activeField.frame, animated: true)
                }
                
            }
        }
    }
    
    //When notification is received that the keyboard will hide (put scrollview back to the top)
    func keyboardHide (_ notification: Notification) {
        let contentInsetsWholeView = UIEdgeInsetsMake(self.topLayoutGuide.length, 0.0, 0.0, 0.0)
        tableViewGrouped.contentInset = contentInsetsWholeView
        tableViewGrouped.scrollIndicatorInsets = contentInsetsWholeView

    }

```
