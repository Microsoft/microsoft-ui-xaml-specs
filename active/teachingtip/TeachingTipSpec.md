# Description

A teaching tip is a semi-persistent and content-rich flyout that provides contextual information. It is often used for informing, reminding, and teaching users about important and new features that may enhance their experience.

**Important APIs:** [TeachingTip class]() [Link TODO]

A teaching tip may also be light-dismiss enabled or used without a pointer indicating reference to any specific UI element. 


### Is this the right control? 

Use a **TeachingTip** control to focus a user's attention on new or important updates and features, remind a user of nonessential options that would improve their experience, or teach a user how a task should be completed. 

Because teaching tip is transient, it would not be the recommended control for prompting users about errors or important status changes.

<!-- Explain why TT is not recommended for notifications --> 


# Examples

The teaching tip has several states, including these notable ones.

A teaching tip can point to a specific UI element to enhance contextual clarity of the information it is presenting. 

![A sample app with a teaching tip pointing at the save button. The tip title reads "Saving automatically" and the subtitle reads "We save your changes as you go - so you never have to." There is a close button on the top right corner of the teaching tip.](TeachingTipPointerSample.jpg)

A teaching tip can be used without a pointer when the information that it is presenting is not specific to a UI element. 

![A sample app with a teaching tip in the bottom right corner. The tip title reads "Saving automatically" and the subtitle reads "We save your changes as you go - so you never have to." There is a close button on the top right corner of the teaching tip.](TeachingTipSampleApp.jpg)

A teaching tip can require manual acknowledgement before dismissing or a teaching tip may also be light-dismiss enabled so that it will dismiss when a user scrolls or interacts with other elements of the application.

![A sample app with a light-dismiss teaching tip in the bottom right corner. The tip title reads "Saving automatically" and the subtitle reads "We save your changes as you go - so you never have to."](TeachingTipLightDismissSample.jpg)


### Create a teaching tip

<!-- Use the Password property to get or set the contents of the PasswordBox. You can do this in the handler for the PasswordChanged event to perform validation while the user enters the password. Or, you can use another event, like a button Click, to perform validation after the user completes the text entry. -->

Here's the XAML for a teaching tip control that demonstrates the default look of the TeachingTip with a title and subtitle. 

When the user clicks the button, a teaching tip will appear and display a message to the user. 

XAML
```XAML
<Button Content="Save" Click="OnFirstSaveButtonClick" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</muxc:TeachingTip>
```

C#
```C#
public void OnFirstSaveButtonClick(object sender, RoutedEventArgs args)
{
   AutoSaveTip.IsOpen = true;
}
```

Here's the result when this code runs and the user clicks the button.

![A sample app with a teaching tip in the bottom right corner. The tip title reads "Saving automatically" and the subtitle reads "We save your changes as you go - so you never have to." There is a close button on the top right corner of the teaching tip.](TeachingTipSampleApp.jpg)

### Add a margin 

A teaching tip can have a margin added by setting the Margin property. When a teaching tip is attached to a UI element, a margin will move it away from the UI element it is pointing to. When a teaching tip is not attached to anything, a margin will move it away from the side of the app window it is positioned towards. 

XAML
```XAML
<Button Content="Save" Click="OnFirstSaveButtonClick" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    Margin="80">
</muxc:TeachingTip>
```

C#
```C#
public void OnFirstSaveButtonClick(object sender, RoutedEventArgs args)
{
   AutoSaveTip.IsOpen = true;
}
```

![A sample app with a teaching tip positioned toward, but not fully against, the bottom right corner. The tip title reads "Saving automatically" and the subtitle reads "We save your changes as you go - so you never have to." There is a close button on the top right corner of the teaching tip.](TeachingTipMarginSample.jpg)

### Point to a UI element

You can also make a teaching tip point to a UI element. This is accomplished by adding the teaching tip to the UI element's resource dictionary and binding the teaching tip's target to that UI element. PointerMode can be used to remove the teaching tip's pointer while keeping its position relative to the targeted UI element.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" Click="OnFirstSaveButtonClick">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            Margin="12"/>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

C#
```C#
public void OnFirstSaveButtonClick(object sender, RoutedEventArgs args)
{
   TeachingTip.SetAttach(SaveButton, AutoSaveTip);
   AutoSaveTip.IsOpen = true;
}
```

![A sample app with a teaching tip pointing at the save button. The tip title reads "Saving automatically" and the subtitle reads "We save your changes as you go - so you never have to." There is a close button on the top right corner of the teaching tip.](TeachingTipPointerSample.jpg)

### Add content

Content can be added to a teaching tip using the Content property. If there is more content to show than what the size of a teaching tip will allow, a scrollbar will be automatically enabled to allow a user to scroll the content area. 

XAML
```XAML
<Button Content="Tip Example" Click="OnFirstSaveButtonClick">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            Margin="12">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

C#
```C#
public void OnFirstSaveButtonClick(object sender, RoutedEventArgs args)
{
    if (!HidingTips)
    {
        AutoSaveTip.IsOpen = true;
    }
}
```

![A sample app with a teaching tip pointing at the save button. The tip title reads "Saving automatically" and the subtitle reads "We save your changes as you go - so you never have to." In the content area of the teaching tip is a CheckBox labeled "Don't show tips at startup" and underneath is text that reads "You can change your tip preferences in Settings if you change your mind" where "Settings" is a link to the app's settings page. There is a close button on the top right corner of the teaching tip.](TeachingTipContentSample.jpg)

### Add buttons

A standard "X" close button is automatically added next to the title of a teaching tip. However, a custom close button can be added to the bottom of a teaching tip by setting the CloseButtonText property. 

**Note: no close button will appear on light-dismiss enabled tips**

A custom action button can be added by setting ActionButtonText, ActionButtonCommand and the ActionButtonCommandParameter properties. 

XAML
```XAML
<Button Content="Tip Example" Click="OnFirstSaveButtonClick">
    <Button.Resources> 
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            ActionButtonText="Disable"
            ActionButtonCommand="DisableAutoSave"
            CloseButtonText="Got it!"
            Margin="12">
                <CheckBox Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                <p>You can change your tip preferences in <a href="app:/item/SettingsPage">Settings</a> if you change your mind.</p>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

C#
```C#
public void OnFirstSaveButtonClick(object sender, RoutedEventArgs args)
{
    if (!HidingTips)
    {
        AutoSaveTip.IsOpen = true;
    }
}
```

![A sample app with a teaching tip pointing at the save button. The tip title reads "Saving automatically" and the subtitle reads "We save your changes as you go - so you never have to." In the content area of the teaching tip is a CheckBox labeled "Don't show tips at startup" and underneath is text that reads "You can change your tip preferences in Settings if you change your mind" where "Settings" is a link to the app's settings page. At the bottom of the teaching are two buttons, a gray one on the left that reads "Disable" and a blue one on the right that reads "Got it!"](TeachingTipButtonsSample.jpg)

### Hero content

Edge to edge content can be added to a teaching tip by setting the HeroContent property. The location of hero content can be set to the top or bottom of a teaching tip by setting the HeroContentPlacement property.

XAML
```XAML
<Button Content="Tip Example" Click="OnFirstSaveButtonClick">
    <Button.Resources> 
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            <muxc:TeachingTip.HeroContent>
                <Image Source="Assets/Giraffe.png" />
            </muxc:TeachingTip.HeroContent>
            Margin="12">
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

C#
```C#
public void OnFirstSaveButtonClick(object sender, RoutedEventArgs args)
{
    TeachingTipExample.IsOpen=true;
}
```

![A sample app with a teaching tip pointing at the save button. The tip title reads "Saving automatically" and the subtitle reads "We save your changes as you go - so you never have to." At the bottom of the teaching tip is a border-to-border picture of a giraffe. There is a close button on the top right corner of the teaching tip.](TeachingTipHeroContentSample.jpg)

### Add an icon

An icon can be added beside the title and subtitle using the IconSource property. Recommended icon sizes include 16px, 24px, and 32px. 

XAML
```XAML
<Button Content="Tip Example" Click="OnFirstSaveButtonClick">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            IconSource="SaveIcon.png"
            Margin="12">
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

C#
```C#
public void OnFirstSaveButtonClick(object sender, RoutedEventArgs args)
{
    TeachingTipExample.IsOpen=true;
}
```

![A sample app with a teaching tip pointing at the save button. The tip title reads "Saving automatically" and the subtitle reads "We save your changes as you go - so you never have to." To the left of the title and subtitle is a floppy disk icon. There is a close button on the top right corner of the teaching tip.](TeachingTipIconSample.jpg)

### Enable light-dismiss

A teaching tip may be light-dismiss enabled so that it will dismiss when a user scrolls or interacts with other elements of the application. The close button will be automatically removed from a light-dismiss teaching tip to indentify its light-dismiss behavior to users. 

XAML
```XAML
<Button Content="Tip Example" Click="OnFirstSaveButtonClick" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True"
    Margin="12">
</muxc:TeachingTip>
```

C#
```C#
public void OnFirstSaveButtonClick(object sender, RoutedEventArgs args)
{
    TeachingTipExample.IsOpen=true;
}
```

![A sample app with a light-dismiss teaching tip in the bottom right corner. The tip title reads "Saving automatically" and the subtitle reads "We save your changes as you go - so you never have to."](TeachingTipLightDismissSample.jpg)

### Preferred placement

Teaching tip replicates all of Flyout's FlyoutPlacementMode placement behavior with the TeachingTipPlacementMode property. As with Flyout, if the preferred placement mode would not leave room for the teaching tip to show, another placement mode will be automatically chosen. 

XAML
```XAML
<Button Content="Tip Example" Click="OnFirstSaveButtonClick" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="TopEdgeAlignedLeft"
    Margin="20,50,0,0"/>
</muxc:TeachingTip>
```

C#
```C#
public void OnFirstSaveButtonClick(object sender, RoutedEventArgs args)
{
    TeachingTipExample.IsOpen=true;
}
```

![A sample app with a teaching tip in the top left corner. The tip title reads "Saving automatically" and the subtitle reads "We save your changes as you go - so you never have to." There is a close button on the top right corner of the teaching tip.](TeachingTipPreferredPlacementSample.jpg)

### Escaping window bounds

On Windows 19H1 and above, a teaching tip can escape window bounds by setting the ShouldConstrainToRootBounds property.

XAML
```XAML
<Button Content="Tip Example" Click="OnFirstSaveButtonClick">

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomEdgeAlignedRight"
    Margin="-80,-50,0,0"
    ShouldConstrainToRootBounds="False">
</muxc:TeachingTip>
```

C#
```C#
public void OnFirstSaveButtonClick(object sender, RoutedEventArgs args)
{
    TeachingTipExample.IsOpen=true;
}
```

![A sample app with a teaching tip outside of the app's bottom right corner. The tip title reads "Saving automatically" and the subtitle reads "We save your changes as you go - so you never have to." There is a close button on the top right corner of the teaching tip.](TeachingTipOutOfWindowBoundsSample.jpg)

### Deferred close

The close event of a teaching tip can be deffered to allow time for an action or custom animation to occur. 

XAML
```XAML
<Button Content="Tip Example">
    <muxc:TeachingTip.Attach> 
        <muxc:TeachingTip x:Name="EnableNewSettingsTip"
            Title="Title string"
            Subtitle="Body text in a minimum height tip."
            Closing="OnTipClosing">
        </muxc:TeachingTip>
    </muxc:TeachingTip.Attach>
</Button>
```

C#
```C#
public void OnTipClosing(object sender, TeachingTipClosingEventArgs args)
{
    if (args.Reason == TeachingTipCloseReason.CloseButton)
    {
        using(args.GetDeferral())
        {
            bool success = await UpdateUserSettings(User thisUsersID);
            if(!success)
            {
                //We were not able to update the settings! Don't close the tip and display the reason why.
                args.Cancel = true;
                ShowLastErrorMessage();
            }
        }
    }
}
```

# Remarks

### Related articles 

* [Dialogs and flyouts](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/index)

### Recommendations
* Tips are impermanent and should not contain information or options that are critical to the experience of an application. 
* Try to avoid showing teaching tips too often. Teaching tips are most likely to each recieve individual attention when they are staggered throughout long sessions or across multiple sessions.    
* Keep tips succint and their topic clear. Research shows users, on average, only read 3-5 words and only comprehend 2-3 words before deciding whether to interact with a tip. 

# API Notes

### Notable Properties  

| Name | Description |
|:-:|:--|
| ShouldConstrainToRootBounds | Gets or sets a value that indicates whether the teaching tip will constrain to the bounds of its root. |
| PreferredPlacement  | Gets or sets the default placement to be used for the teaching tip, in relation to its placement target if targeted. |

### Methods   

| Name | Description |
|:-:|:--|
| SetAttach | Associates the specified teaching tip with the specified UIElement. |
| GetAttach | Gets the teaching tip associated with the specified element. |

### Events    

| Name | Description |
|:-:|:--|
| ActionButtonClick | Occurs after the action button has been tapped. |
| Closing |Occurs just before the tip begins to close. |
| Closed | Occurs after the tip is closed. |
| CloseButtonClick | Occurs after the close button has been tapped. |

# API Details 

```c++ 
enum TeachingTipCloseReason
{
    CloseButton,
    LightDismiss,
    Programmatic,
};

enum TeachingTipPlacementMode
{
    Auto,
    Top,
    Bottom,
    Left,
    Right,
    TopEdgeAlignedRight,
    TopEdgeAlignedLeft,
    BottomEdgeAlignedRight,
    BottomEdgeAlignedLeft,
    LeftEdgeAlignedTop,
    LeftEdgeAlignedBottom,
    RightEdgeAlignedTop,
    RightEdgeAlignedBottom,
};

enum TeachingTipHeroPlacementMode
{
    Top,
    Bottom,
    Auto,
};

enum TeachingTipPointerMode
{
    Auto,
    On,
    Off,
};

runtimeclass TeachingTipClosedEventArgs
{
    TeachingTipCloseReason Reason{ get; };
};

runtimeclass TeachingTipClosingEventArgs
{
    TeachingTipCloseReason Reason{ get; };
    Boolean Cancel;
    Windows.Foundation.Deferral GetDeferral();
};

unsealed runtimeclass TeachingTipTemplateSettings : Windows.UI.Xaml.DependencyObject
{
    TeachingTipTemplateSettings();

    Windows.UI.Xaml.Thickness TopRightHighlightMargin;
    Windows.UI.Xaml.Thickness TopLeftHighlightMargin;

    static Windows.UI.Xaml.DependencyProperty TopRightHighlightMarginProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty TopLeftHighlightMarginProperty{ get; };
}

unsealed runtimeclass TeachingTip : Windows.UI.Xaml.Controls.ContentControl
{
    TeachingTip();

    String Title;
    String Subtext;

    Boolean IsOpen;

    String ActionButtonText;
    Windows.UI.Xaml.Style ActionButtonStyle;
    Windows.UI.Xaml.Input.ICommand ActionButtonCommand;
    Object ActionButtonCommandParameter;

    String CloseButtonText;
    Windows.UI.Xaml.Style CloseButtonStyle;
    Windows.UI.Xaml.Input.ICommand CloseButtonCommand;
    Object CloseButtonCommandParameter;

    Windows.UI.Xaml.Thickness TargetOffset;
    Boolean IsLightDismissEnabled;
    TeachingTipPlacementMode PreferredPlacement;
    TeachingTipHeroPlacementMode HeroPlacement;
    TeachingTipPointerMode PointerMode;

    Windows.UI.Xaml.UIElement HeroContent;
    IconSource IconSource;

    TeachingTipTemplateSettings TemplateSettings{ get; };

    event Windows.Foundation.TypedEventHandler<TeachingTip, Object> ActionButtonClick;
    event Windows.Foundation.TypedEventHandler<TeachingTip, Object> CloseButtonClick;
    event Windows.Foundation.TypedEventHandler<TeachingTip, TeachingTipClosingEventArgs> Closing;
    event Windows.Foundation.TypedEventHandler<TeachingTip, TeachingTipClosedEventArgs> Closed;

    static Windows.UI.Xaml.DependencyProperty IsOpenProperty{ get; };

    static Windows.UI.Xaml.DependencyProperty AttachProperty{ get; };
    static void SetAttach(Windows.UI.Xaml.UIElement element, TeachingTip teachingTip);
    static TeachingTip GetAttach(Windows.UI.Xaml.UIElement element);

    static Windows.UI.Xaml.DependencyProperty TitleProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty SubtextProperty{ get; };

    static Windows.UI.Xaml.DependencyProperty ActionButtonTextProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty ActionButtonStyleProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty ActionButtonCommandProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty ActionButtonCommandParameterProperty{ get; };

    static Windows.UI.Xaml.DependencyProperty CloseButtonTextProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty CloseButtonStyleProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty CloseButtonCommandProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty CloseButtonCommandParameterProperty{ get; };

    static Windows.UI.Xaml.DependencyProperty TargetOffsetProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty IsLightDismissEnabledProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty PreferredPlacementProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty HeroPlacementProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty PointerModeProperty{ get; };

    static Windows.UI.Xaml.DependencyProperty HeroContentProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty IconSourceProperty{ get; };

    static Windows.UI.Xaml.DependencyProperty TemplateSettingsProperty{ get; };
}
```

# Appendix

### Visual Components

 | Component |  Notes |
|:---:|:---|
| Container | * The container is the body of the tip and encapsulates all the tip components. <br> * Nonmodal. <br> * If content height exceeds max height or width, vertical scrolling will be enabled. See Scroll. <br> * For visibility concerns, the container has a border around the outer edge, which adheres to the pointer if present. See Pointer. <br> * For visibility concerns, the top edge of the container has a 1px highlight which also adheres to the pointer if present. See Pointer. <br><br> ![An empty teaching tip.](Container.PNG) |
| Title| * Semi-bolded. <br> * Text wraps at Close (X) Button and Container border. <br><br> ![A teaching tip with a title and subtitle populated with sample text.](Title.PNG) |
| Subtitle | * Text wraps at Close (X) Button and Container border. <br><br> ![A teaching tip with a subtitle that reads "Body text in a minimum height tip.](Subtitle.PNG) |
| Content | * Can be customized to include text, images, videos, animation, checkboxes, hyperlinks, and any other XAML content. <br> * Will scroll if there is more content to show than tip height allows. See Scroll Bar. <br> * Placed below Subtitle and above Close/Action Button. <br><br> ![A teaching tip with a sample title and subtitle. The teaching tip has a picture of a mountain in the content area.](Content.PNG) |
| Close Button | * Will appear as an X Button in the top right corner by default and in the top left corner automatically for RTL languages. The close button may also be set to appear in the bottom right corner of the tip as a traditional button or be set to not show at all so that a custom close option may be implemented in the Content Area. <br> * If a tip is set to light-dismiss, no close button will appear at all. <br><br> ![A teaching tip with a sample title and subtitle. There is a close button in the top right corner.](HeaderClose.PNG) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![A teaching tip with a sample title and subtitle. In the teaching tip's content area is a CheckBox labeled "Don't show tips at startup. At the bottom of the teaching tip is a button that reads "Got it".](FooterClose.PNG) |
| Action Button | * Allows the user to invoke a custom event. <br> * This is the only non-Close button provided out of the box. Other buttons may be implemented in the Content Area. <br><br> ![A teaching tip with a sample title and subtitle. At the bottom of the tip are two buttons, a blue button on the left that reads "Default" and a gray button on the right that reads "Secondary".](ActionButton.PNG) |
| Pointer | * Triangular extension of the tip body that can be used to indicate that the tip is referring to on-screen UI element. <br> * When PointerMode is set to auto, the pointer will automatically appear on tips that are attached to a target and off for tips that are not attached to a target. <br> * Prefers to center on target. <br> * Maintains a 12px margin from edges of the tip. <br> * Not animated. <br> * Will not have a shadow as shadows cannot yet be added to nonrectangular surfaces. ![An empty teaching tip with a triangular pointer centered outward on its bottom edge.](Pointer.PNG) |
| Icon | * Added to the left of the Title and Subtitle by default and automatically moved to the right for RTL languages. <br><br> ![A teaching tip with a sample title and subtitle. To the left of the title is a sample icon showing a graph of connected dots.](Icon.PNG) |
| Hero Content | * Hero Content is media that stretches to the edges of a tip. <br> * Can be placed at the top or bottom of a tip. <br><br> ![A teaching tip with a sample title and subtitle. Above the title and subtitle is a large border-to-border picture of a giraffe.](HeroContent.PNG) |
| Scroll Bar | * If the tip's contents are large enough to require scrolling, a scrollbar which will not intersect the Close (X) Button will be added to the content area. <br><br> ![A tall teaching tip with a scrollbar on the right a long string of "Lorem ipsum" text in the body.](ScrollBar.PNG) |

### Behavioral Components

 | Property | Notes |
|:---:|:---|
| Opening | * A tip is shown by setting its IsOpen property to true. <br> * Tips will animate on opening. <br> * When a tip does not have enough available window space to fully show in any location [see Placement], it will not open and will instead overwrite IsOpen to false. |
| Closing | * There are three ways to close a tip: set the IsOpen property to false, the user invokes a close button, or the tip is closed via light dismiss. These will return the method used in TeachingTipCloseReason.  <br> * Closing can be deferred by taking a handle to the deferral object in the closing event args. |
| Placement | * Placement modes for targeted tips will follow the precedent of Flyout. Full placement mode will be replaced by Center which positions Pointer at the center of the element. <br> * Placement modes for untargeted tips will include each side, corner, and center of the application window. <br> * The following properties are not preferred in tip placement: <br> &nbsp;&nbsp;&nbsp;&nbsp; * There is not enough space for the tip to show without clipping. <br> &nbsp;&nbsp;&nbsp;&nbsp; * The target is not large enough to maintain the tip's alignment and the Pointer's 12px margin from the edge of the tip. <br> &nbsp;&nbsp;&nbsp;&nbsp; * The target element is too large to maintain edge alignment while keeping the Pointer centered on the target. |
| Light-dismiss | * Allows a tip to be dismissed when the user scrolls or clicks elsewhere within the application. <br> * **TODO:** Work with Accessibility and Design to create a timed fade-out that would allow users to recover a dismissing tip via click or cursor hover. |
| Persistent Tip Location | * Once a tip is open, it will not move even if its target does. The exception to this is when the window is resized. |
| Motion | * Tips have built in open and close animations that can be customizable using Storyboards.|
| Pointer/Hero Content Avoidance | * To avoid the visual oddity of the Pointer emerging from Hero Content, the Pointer and Hero Content will attempt to avoid intersecting using the following rules: <br> &nbsp;&nbsp;&nbsp;&nbsp; * Move the hero content to Top or Bottom (Disabled when HeroContentPlacement is not auto). <br> &nbsp;&nbsp;&nbsp;&nbsp; * Shift the beak along the edge of the tip (Disabled when the placement of the tip is edge aligned). <br> &nbsp;&nbsp;&nbsp;&nbsp; * Change the placement of the tip (Disabled when the tip placement is not auto).  |
| Out of Window Bounds | * Tips can escape window bounds on newer OS versions via the ShouldConstrainToRootBounds property.  When this property is set to false, the tip uses screen boundaries instead of window boundaries during its placement algorithm. |

### Accessibility

| State | Action | Narrator |
|:---|:---|:---|
| Tip first appears and is not in focus | No action is needed invoke the tip. | “New tip. It says …." | 
| Tip is first focused on by tabbing through to the tip | Targeted Tip: <br> Tip is injected to tab stop right after its target element. User tabs to reach tip and put it into focus. <br><br> Untargeted Tip: <br> Tip is injected to tab stop as the last item to be created. User tabs to reach tip and put it into focus. <br><br> Light-dismiss: <br> Automatically take focus. | Default Tab Stop, Title, and Subtitle. <br><br> Ex: “X Button, Tip Title, Tip Subtitle.” |
| Tip is tabbed through | Tab Button: <br> Will go through all actionable items regardless of group in order. When tab is pressed on the last element in the teaching tip, focus will cycle to the first element in the teaching tip.  <br> <br> Arrow keys: <br> Will be able to explore groups in the specified directions. <br> <br> Enter and Escape: <br> Will result in closing the tip. <br> <br> Spacebar: <br> Will select the component focused on. <br> <br> Home and End: <br> Do not have use unless within a XAML component that supports their functionality | Name of XAML or visual component. Ex:<br><br> “X Button” <br><br> “Save Button” <br><br> “Learn More Hyperlink” | 
| Tip is dismissed | 1. X Button is pressed. <br> 2. Close Button is pressed. <br> 3. Action Button is pressed. <br> Tab focus goes back to where it should be, the predecessor. | Nothing | 


### Data and Intelligence Metrics

##### P0: Feature Key Performance Indicators

Feature has received enough app/developer validation and feedback for it to exit preview.

* KPI: Three or more first party applications are validating the feature in prerelease.
    * Measurement: Occurrence of TeachingTip type in apps on developer devices via telemetry (running under debugger). **Note for telemetry sampling considerations:** Need to be cautious for WinUI as it does not work the same way as with OS SDK. If the solution is not carefully considered, it will result in too little dev data (at 2% sampling) or too much end-user data (at 100% sampling) and hence throttling (telemetry data loss). Requires further investigation. 
    * Measurement: Count of users providing feedback on GitHub/UserVoice/Feedback Hub/SIUF.
    
Developers feel delighted and satisfied with the feature. 

* KPI: Dev NSAT is positive (min), >20 (aspirational) as we exit 2019.
    * Measurement: Developer survey via SIUF, e.g.: "How useful is the feature?" 

##### P1: Feature Performance Indicators

Ratio of apps applying teaching tip should be comparable across all Windows experiences.

* KPI: Ratio of apps using teaching tip per Windows experience and form factor is comparable as we exit 2019. 
    * Measurement: Occurrence of TeachingTip type in apps via static analysis and telemetry.

* KPI: Average time before teaching tip is dismissed by end user per Windows experience and form factor is comparable.
    * Measurement: Time delta between TeachingTip.IsOpen = true/false via telemetry.

Uses of teaching tip are perceived useful and are not an annoyance for end users.

* KPI: Average display time for teaching tip is between 3 and 30 seconds, excluding instances containing video media.
    * Measurement: Time delta between TeachingTip.IsOpen = true/false via telemetry for tips not containing video media. 
  
##### P2+: Trailing Indicators
 
N/A
