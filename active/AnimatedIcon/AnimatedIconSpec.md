
# AnimatedIcon

# Background

Xaml has an
[IconElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.IconElement)
base type today with derived types like
[BitmapIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.BitmapIcon)
and [SymbolIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SymbolIcon).
Since it's an element it can be used in the UI tree, such as in a 
[StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel).

All of these icons are static though, and developers do not have a straightforward way to
add an animated icon in controls where they can use a static icon.
There is also a desire to have an animated icon respond to user interactivity via state changes to either draw
attention to an action a user can take or to add user delight.

The [AnimatedVisualPlayer](https://docs.microsoft.com/uwp/api/Microsoft.UI.Xaml.Controls.AnimatedVisualPlayer)
element can be used to create animated (Lottie-based) that are small like an icon, but:
* to have the animation be driven by the states of and interactions with a control,
developers may need to write a lot of custom code.
* AnimatedVisualPlayer does not inherit from IconElement, so it cannot be used in places that
need one, such as
[NavigationViewItem.Icon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationViewItem.Icon)
or in a DropDownButton.

This spec introduces an AnimatedIcon IconElement which reduces, or ideally eliminates, the amount of code-behind needed to
implement most scenarios, and it can be used in WinUI controls that support an IconElement.

Along with the various IconElement-based there are equivalent IconSource-source types.
For example BitmapIcon (IconElement) and
[BitmapIconSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.BitmapIconSource)
(IconSource).
Similarly in this spec, along with `AnimatedIcon` is an `AnimatedIconSource`.

# AnimatedIcon How-To

## Creating animated content with Lottie

Defining an animation for an AnimatedIcon[Source] is the same as the process for an
[AnimatedVisualPlayer](http://msdn.microsoft.com/library/Microsoft.UI.Xaml.Controls.AnimatedVisualPlayer).
You will need to download the Lottie file for the icon you are looking to add and follow the steps in
[this]((https://docs.microsoft.com/en-us/windows/communitytoolkit/animations/lottie-scenarios/getting_started_codegen))
tutorial to run that file through LottieGen.
The output of LottieGen will produce the file needed for AnimatedIcon.

The additional step for AnimatedIcon is to define markers in the animation definition.
These can be used by controls to transition the animation in the different control states.
So the output from the LottieGen tool will also include the markers needed for animated icon to
map the correct state transitions tothe correct animation segments.
It will also contain the themable color properties you can set in code. 

## Design Requirements  

Designers will need to add markers named like the following to all Lottie files that will be used with controls that support AnimatedIcon. 
The controls that currently support AnimatedIcon are listed in the next section. 

The marker names should reflect the visual state the animation is going from to the visual state the animation is
going to. For example, the bare minimum of the markers that are needed for icons being used in a NavigationViewItem are:  

* NormalToPointerOver 
* NormalToPressed 
* PointerOverToNormal 
* PointerOverToPressed 
* PressedToNormal 
* PressedToPointerOver  

“Normal" is the state where the user is not interacting with the NavigationViewIten. 

Markers will need to be named this specific way so AnimatedIcon can map the correct animation segment with
the correct visual state transition.
This solution is conforming to a standard set of strings that are already defined for many Xaml controls.
Here is an example of how the markers would look in a Lottie JSON file for the NavigationViewItem.  

"markers":[{"tm":0,"cm":"NormalToPointerOver_Start","dr":0},{"tm":9,"cm":"NormalToPointerOver_End","dr":0}, 

{"tm":10,"cm":"NormalToPressed_Start","dr":0},{"tm":19,"cm":"NormalToPressed_End","dr":0}, 

{"tm":20,"cm":"PointerOverToNormal_Start","dr":0},{"tm":29,"cm":"PointerOverToNormal_End","dr":0}, 

{"tm":30,"cm":"PointerOverToPressed_Start","dr":0},{"tm":39,"cm":"PointerOverToPressed_End","dr":0}, 

{"tm":40,"cm":"PressedToNormal_Start","dr":0},{"tm":49,"cm":"PressedToNormal_End","dr":0}, 

{"tm":50,"cm":"PressedToPointerOver_Start","dr":0},{"tm":69,"cm":"PressedToPointerOver_End","dr":0}, 

{"tm":90,"cm":"PressedToNormal_Start","dr":0},{"tm":99,"cm":"PressedToNormal_End","dr":0}, 

{"tm":100,"cm":"PressedToPointerOver_Start","dr":0},{"tm":101,"cm":"PressedToPointerOver_End","dr":0}] 

## Controls that support AnimatedIcon

Many built-in Xaml controls support AnimatedIcon[Source] by animating to markers when the state of the control changes.
This gives you the ability to add an animated icon with the correct markers in the file,
to one of the controls without needing to do any more work. The types of controls that are supported for V1 are defined below. 

### Supported controls that take an IconElement or IconSource

Some controls take an IconElement or IconSource in the control so the developer can add a custom icon to the control. 
We updated the default templates for two of these controls to support an AnimatedIcon so the developer would only need to add 
the animated icon code to the content of the control. The control that supports this for V1 is NavigationViewItem. 

We plan to update more controls for V2+ once we see how the community is using this control and we learn about more use cases. 

* NavigationViewItem

See the example titled "Swap out a static icon with an animated icon in a Navigation View Item" below for more information. 

### Controls whose templates have been updated 

We have also updated the default templates for a handful of controls that currently show a static icon so a developer can update the template 
and use an animated icon instead. For V1, here are the control templates we are going to update:

* AutoSuggestBox
* CheckBox
* DropDownButton
* Expander
* SplitButton
* Button

See the examples titled "Add an animated icon to a control that has an icon by default" to see where to update the template with the AnimatedIcon code. 

### Default markers for animations in supported controls

AnimatedIcon requires the icon JSON files to include markers named in a standard way so the control can successfully map the visual state transition to the 
correct animation segment. You can see the design requirements section for more specifics on the structure of the markers and the naming. The list below shows the marker names that 
should be in the animation file in order for the supported controls to play the animation correctly. 

Here are the default states for each of the controls:

* AutoSuggestBox 
    * NormalToPointerOver 
    * NormalToPressed 
    * PointerOverToNormal 
    * PointerOverToPressed 
    * PressedToNormal 
    * PressedToPointerOver 
* Button
    * NormalToPointerOver 
    * NormalToPressed 
    * PointerOverToNormal 
    * PointerOverToPressed 
    * PressedToNormal 
    * PressedToPointerOver  
* CheckBox 
    * NormalOnToPointerOverOn
    * NormalOnToPressedOn
    * NormalOffToNormalOn
    * NormalOffToPointerOverOff
    * NormalOffToPressedOff
    * PointerOverOnToPointerOverOff
    * PointerOverOnToNormalOn
    * PointerOverOnToPressedOn
    * PointerOverOffToPointerOverOn
    * PointerOverOffToNormalOff
    * PointerOverOffToPressedOff
    * PressedOnToPressedOff
    * PressedOnToPointerOverOff
    * PressedOnToNormalOff_Start
    * PressedOffToPressedOn
    * PressedOffToPointerOver
    * PressedOffToNormalOn
* Expander 
    * NormalToPointerOver 
    * NormalToPressed 
    * PointerOverToNormal 
    * PointerOverToPressed 
    * PressedToNormal 
    * PressedToPointerOver  
* DropDownButton 
    * NormalToPointerOver 
    * NormalToPressed 
    * PointerOverToNormal 
    * PointerOverToPressed 
    * PressedToNormal 
    * PressedToPointerOver  
* NavigationViewItem
    * NormalToPointerOver 
    * NormalToPressed 
    * NormalToSelected
    * NormalToPointerOverSelected
    * NormalToPressedSelected
    * PointerOverToNormal 
    * PointerOverToPressed 
    * PointerOverToSelected
    * PointerOverToPointerOverSelected
    * PointerOverToPressedSelected
    * PressedToNormal 
    * PressedToPointerOver 
    * PressedToSelected
    * PressedToPointerOverSelected
    * PressedToPressedSelected
    * SelectedToPointerOverSelected
    * SelectedToPressedSelected
    * SelectedToNormal
    * SelectedToPointerOver
    * SelectedToPressed
    * PointerOverSelectedToPressedSelected
    * PointerOverSelectedToSelected
    * PointerOverSelectedToNormal
    * PointerOverSelectedToPointerOver
    * PointerOverSelectedToPressed
    * PressedSelectedToPointerOverSelected
    * PressedSelectedToSelected
    * PressedSelectedToNormal
    * PressedSelectedToPointerOver
    * PressedSelectedToPressed
* SplitButton 
    * NormalToPointerOver 
    * NormalToPressed 
    * PointerOverToNormal 
    * PointerOverToPressed 
    * PressedToNormal 
    * PressedToPointerOver  



# API Pages

## AnimatedIcon class

An AnimatedIcon is a UI component that has APIs that enables the playing of animated images in response to user interaction and visual state changes. Some WinUI controls will call these APIs in response to visual state changes made by the user interacting with the control. 

On Windows, the VisualStateManager is used to manage the logic for transitioning between states for controls.
Each control has a specified VisualStateGroup, which contains mutually exclusive VisualState objects.
For example, a DropDownButton may have a VisualStateGroup called “CommonStates", which specifies a VisualState object for
“Normal" “Pressed", “PointerOver", and “Disabled".

A control will use the VisualStateManager to determine what state the animated icon is transitioning to and
play the corresponding animation segment of the icon.
The icon code will need to have markers named in a standard way (described below) so
AnimatedIcon can map the correct animation segment with the visual state the control is transitioning to.  

**Important APIs:** [AnimatedIcon class](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.AnimatedIcon)

### Is this the right control? 

Use an AnimatedIcon control when you want to tie an animated icon to a user interaction in your UI.
Do not use an AnimatedIcon control if you want to just play an animation once or control the playback of the animation.
Do not use an AnimatedIcon control if you want to play your animation in a control that does not support IconElement.
Instead, use the AnimatedVisualPlayer control. 

An AnimatedIcon control can be used anywhere an IconElement can be used in WinUI. 

### How is this different than [AnimatedVisualPlayer](https://docs.microsoft.com/uwp/api/Microsoft.UI.Xaml.Controls.AnimatedVisualPlayer)?

AnimatedIcon is an
[IconElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.IconElement),
and so can be used anywhere an element or IconElement is required, such as
[NavigationViewItem.Icon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationViewItem.Icon).
AnimatedVisualPlayer is a more  general animation player that can be used anywhere in an application where AnimatedIcon is intended to only be used where icons are used in WinUI. 

AVP allows developers to control animations using the following methods: Pause, PlayAsync, Resume, SetProgress, and Stop. When using AVP, developers may need to write a lot of code when getting their generated animation to properly respond to user interaction and state changes. This is the main distinction between the workflows for AVP and AnimatedIcon. An AnimatedIcon will be used when animations need to be driven by state changes and user interaction in a markup friendly way. It would reduce, or ideally eliminate the amount of code-behind needed to implement such scenarios.

### Examples

#### Swap out a static icon with an animated icon in a Navigation View Item

NavigationViewItem has states for rest/pointer over/press and AnimatedIcon will
animate the state transitions by having named markers in the Lottie file such as:  

- NormalToPointerOver
- NormalToPressed
- PointerOverToNormal
- PointerOverToPressed
- PressedToNormal
- PressedToPointerOver

Below is an example of how to add an animated play icon with the standard state transitions mentioned above to
a Hierarchical NavigationView item.

AnimatedIcon requires you to run the Lottie JSON file through the Windows [CodeGen](https://docs.microsoft.com/en-us/windows/communitytoolkit/animations/lottie-scenarios/getting_started_codegen) tool to generate the animation code as a C# class. This process is similar to the process needed for AnimatedVisualPlayer. AnimatedIcon needs the JSON file to run through CodeGen so the markers and color properties are translated in the format needed for the control to map the values correctly. 

Once you run your Lottifile through codegen you can add the output class to your project. Follow the steps in the [CodeGen](https://docs.microsoft.com/en-us/windows/communitytoolkit/animations/lottie-scenarios/getting_started_codegen) tutorial for more instructions. 

Once the icon file is added to your project you can then add the icon to your navigation view item like you would any other icon type that inherits from IconElement. 

Before: 

XAML

```xml
<muxc:NavigationView.MenuItems>
    <muxc:NavigationViewItem Content = "Menu Item1" Icon ="Play" Tag="SamplePage" x:Name = "SamplePageItem">
</muxc:NavigationView.MenuItems>
```

After: 

XAML

```xml
<muxc:NavigationView.MenuItems >
    <muxc:NavigationViewItem Content = "Menu Item1" Tag="SamplePage" x:Name = "SamplePageItem" >
        <muxc:NavigationViewItem.Icon >
            <muxc:AnimatedIcon x:Name = "PlayAnimatedIcon" >
                <muxc:AnimatedIcon.Source >
                    < animatedvisuals:PlayIcon />
                </muxc:AnimatedIcon.Source>
            </muxc:AnimatedIcon >
    </muxc:NavigationViewItem.Icon >
</muxc:NavigationView.MenuItems >
```

# Add an animated icon to a control that does not have an updated template for the AnimatedIcon control

If you would like to add an animated icon to a control that does not have the default template updated,
you will need to update the template to include the state changes needed to play the animation segments. 

Here is the XAML for the changes you would need to make to add an AnimatedIcon to a button (button does not have the default template updated to support AnimatedIcon like the example above). Button has a content property which derives from UIElement so you can add an AnimatedIcon to a button but you would need tp update the template first. This example shows how to add the setters for the Normal, PointerOver, Pressed and Disabled states. The following code is added for each of the four visual states: 

XAML
```xml
<VisualState.Setters>
    <Setter Target="ContentPresenter.(AnimatedIcon.State)" Value="Normal"/>
</VisualState.Setters>
```

You will then add the markup to a content presenter in the template for the AnimatedIcon control to present the animated visual glyph. 
XAML 
```xml
<muxc:AnimatedIcon x:Name="AnimatedPlayIcon" Foreground="{TemplateBinding Foreground}" Grid.Column="1">
    <muxc:AnimatedIcon.Source>
        <AnimatedVisuals:PlayIconAnimation/>
    </muxc:AnimatedIcon.Source>
    <muxc:AnimatedIcon.FallbackIconSource>
        <muxc:FontIconSource
            FontFamily="{ThemeResource SymbolThemeFontFamily}"
            Glyph="{StaticResource PlayIconGlyph}"
            FontSize="{StaticResource PlayIconGlyphSize}"
            Foreground="{ThemeResource PlayIconGlyphForegroundUnchecked}"/>
    </muxc:AnimatedIcon.FallbackIconSource>
</muxc:AnimatedIcon>
```

Here is the full template update for the default button style. 

XAML
```xml
 <ControlTemplate TargetType="Button">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Height="*"/>
            <ColumnDefinition Height="*"/>
        </Grid.ColumnDefinitions>
        <contract7Present:ContentPresenter.BackgroundTransition>
            <contract7Present:BrushTransition Duration="0:0:0.083" />
        </contract7Present:ContentPresenter.BackgroundTransition>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState x:Name="Normal"/>
                    <VisualState x:Name="PointerOver">
                        <Storyboard>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource AccentButtonBackgroundPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="BorderBrush">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource AccentButtonBorderBrushPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="Foreground">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource AccentButtonForegroundPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                        </Storyboard>
                        <VisualState.Setters>
                            <Setter Target="AnimatedPlayIcon.(AnimatedIcon.State)" Value="PointerOver"/>
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Pressed">
                        <Storyboard>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource AccentButtonBackgroundPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="BorderBrush">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource AccentButtonBorderBrushPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="Foreground">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource AccentButtonForegroundPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                        </Storyboard>
                        <VisualState.Setters>
                            <Setter Target="AnimatedPlayIcon.(AnimatedIcon.State)" Value="Pressed"/>
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <Storyboard>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource AccentButtonBackgroundDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="BorderBrush">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource AccentButtonBorderBrushDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="Foreground">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource AccentButtonForegroundDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                        </Storyboard>
                        <VisualState.Setters>
                            <!-- DisabledVisual Should be handled by the control, not the animated icon. -->
                            <Setter Target="AnimatedPlayIcon.(AnimatedIcon.State)" Value="Normal"/>
                        </VisualState.Setters>
                    </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        <ContentPresenter
                x:Name="ContentPresenter"
                Background="{TemplateBinding Background}"
                contract7Present:BackgroundSizing="{TemplateBinding BackgroundSizing}"
                Foreground="{TemplateBinding Foreground}"
                BorderBrush="{TemplateBinding BorderBrush}"
                BorderThickness="{TemplateBinding BorderThickness}"
                Content="{TemplateBinding Content}"
                ContentTemplate="{TemplateBinding ContentTemplate}"
                ContentTransitions="{TemplateBinding ContentTransitions}"
                contract7Present:CornerRadius="{TemplateBinding CornerRadius}"
                contract7NotPresent:CornerRadius="{ThemeResource ControlCornerRadius}"
                Padding="{TemplateBinding Padding}"
                HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}"
                VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"
                AutomationProperties.AccessibilityView="Raw"
                local:AnimatedIcon.State="Normal"/>
                <muxc:AnimatedIcon x:Name="AnimatedPlayIcon" Foreground="{TemplateBinding Foreground}" Grid.Column="1">
                    <muxc:AnimatedIcon.Source>
                        <AnimatedVisuals:PlayIconAnimation/>
                    </muxc:AnimatedIcon.Source>
                    <muxc:AnimatedIcon.FallbackIconSource>
                        <muxc:FontIconSource
                            FontFamily="{ThemeResource SymbolThemeFontFamily}"
                            Glyph="{StaticResource PlayIconGlyph}"
                            FontSize="{StaticResource PlayIconGlyphSize}"
                            Foreground="{ThemeResource PlayIconGlyphForegroundUnchecked}"/>
                    </muxc:AnimatedIcon.FallbackIconSource>
                </muxc:AnimatedIcon>
        </Grid>
    </ControlTemplate>  
```

# Add an animated icon to a control that has an icon by default 

You might want to update a control that already has a static icon in it's default template to an animated icon. This example will show how to update the DropDownButton to use an animated glyph instead of the static chevron icon. 

In this scenario you would need to update the template to add the setters for the visual states defined above in this document for DropDownButton.

XAML
```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup x:Name="CommonStates">
        <VisualState x:Name="Normal">
            <VisualState.Setters>
                <Setter Target="AnimatedChevronIcon.(AnimatedIcon.State)" Value="Normal"/>
            </VisualState.Setters>
        </VisualState>
        <VisualState x:Name="PointerOver">
            <Storyboard>
                <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="Background">
                    <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonBackgroundPointerOver}" />
                </ObjectAnimationUsingKeyFrames>
                <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="BorderBrush">
                    <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonBorderBrushPointerOver}" />
                </ObjectAnimationUsingKeyFrames>
                <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="Foreground">
                    <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonForegroundPointerOver}" />
                </ObjectAnimationUsingKeyFrames>
            </Storyboard>
            <VisualState.Setters>
                <Setter Target="AnimatedChevronIcon.(AnimatedIcon.State)" Value="PointerOver"/>
            </VisualState.Setters>
        </VisualState>

        <VisualState x:Name="Pressed">
            <Storyboard>
                <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="Background">
                    <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonBackgroundPressed}" />
                </ObjectAnimationUsingKeyFrames>
                <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="BorderBrush">
                    <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonBorderBrushPressed}" />
                </ObjectAnimationUsingKeyFrames>
                <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="Foreground">
                    <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonForegroundPressed}" />
                </ObjectAnimationUsingKeyFrames>
                <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ChevronIcon" Storyboard.TargetProperty="Foreground">
                    <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource DropDownButtonForegroundSecondaryPressed}" />
                </ObjectAnimationUsingKeyFrames>
            </Storyboard>
            <VisualState.Setters>
                <Setter Target="AnimatedChevronIcon.(AnimatedIcon.State)" Value="Pressed"/>
            </VisualState.Setters>
        </VisualState>

        <VisualState x:Name="Disabled">
            <Storyboard>
                <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="Background">
                    <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonBackgroundDisabled}" />
                </ObjectAnimationUsingKeyFrames>
                <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="BorderBrush">
                    <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonBorderBrushDisabled}" />
                </ObjectAnimationUsingKeyFrames>
                <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="Foreground">
                    <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonForegroundDisabled}" />
                </ObjectAnimationUsingKeyFrames>
                <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ChevronIcon" Storyboard.TargetProperty="Foreground">
                    <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonForegroundDisabled}" />
                </ObjectAnimationUsingKeyFrames>
            </Storyboard>
            <VisualState.Setters>
                <!-- DisabledVisual Should be handled by the control, not the animated icon. -->
                <Setter Target="AnimatedChevronIcon.(AnimatedIcon.State)" Value="Normal"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

You will then need to replace the TextBlock control with the AnimatedIcon control. 

Before: 

XAML
```xml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="80"/>
</Grid.ColumnDefinitions>

<ContentPresenter x:Name="ContentPresenter"
    Content="{TemplateBinding Content}"
    ContentTransitions="{TemplateBinding ContentTransitions}"
    ContentTemplate="{TemplateBinding ContentTemplate}"
    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}"
    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"
    AutomationProperties.AccessibilityView="Raw" />

<TextBlock
    x:Name="ChevronTextBlock"
    Grid.Column="1"
    FontFamily="{ThemeResource SymbolThemeFontFamily}"
    FontSize="8"
    Foreground="{ThemeResource ButtonForegroundPressed}"
    Text="&#xE96E;"
    VerticalAlignment="Center"
    Padding="2,4,2,0"
    Margin="8,0,0,0"
    IsTextScaleFactorEnabled="False"
    AutomationProperties.AccessibilityView="Raw"/>
</Grid>
```

After:

XAML
```xml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="80"/>
</Grid.ColumnDefinitions>

<ContentPresenter x:Name="ContentPresenter"
    Content="{TemplateBinding Content}"
    ContentTransitions="{TemplateBinding ContentTransitions}"
    ContentTemplate="{TemplateBinding ContentTemplate}"
    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}"
    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"
    AutomationProperties.AccessibilityView="Raw" />

<local:AnimatedIcon x:Name="AnimatedChevronIcon" Grid.Column="1" Margin="10,4,2,0" AutomationProperties.AccessibilityView="Raw">
    <local:AnimatedIcon.Source>
        <AnimatedVisuals:AnimatedChevron/>
    </local:AnimatedIcon.Source>
    <local:AnimatedIcon.FallbackIconSource>
        <local:FontIconSource
            FontSize="8"
            FontFamily="{ThemeResource SymbolThemeFontFamily}"
            Foreground="{ThemeResource ButtonForegroundPressed}"
            Glyph="&#xE96E;"
            IsTextScaleFactorEnabled="False"/>
    </local:AnimatedIcon.FallbackIconSource>
</local:AnimatedIcon>
</Grid>
```

Here is the full file update for DropDownButton. 

XAML

```xml
<Style x:Key="DefaultDropDownButtonStyle" TargetType="local:DropDownButton">
        <Setter Property="Background" Value="{ThemeResource ButtonBackground}" />
        <Setter Property="Foreground" Value="{ThemeResource ButtonForeground}" />
        <Setter Property="BorderBrush" Value="{ThemeResource ButtonBorderBrush}" />
        <Setter Property="BorderThickness" Value="{ThemeResource ButtonBorderThemeThickness}" />
        <Setter Property="Padding" Value="{StaticResource ButtonPadding}" />
        <Setter Property="HorizontalAlignment" Value="Left" />
        <Setter Property="VerticalAlignment" Value="Center" />
        <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}" />
        <Setter Property="FontWeight" Value="Normal" />
        <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
        <Setter Property="UseSystemFocusVisuals" Value="{StaticResource UseSystemFocusVisuals}" />
        <Setter Property="FocusVisualMargin" Value="-3" />
        <contract7Present:Setter Property="CornerRadius" Value="{ThemeResource ControlCornerRadius}" />
        <contract7Present:Setter Property="BackgroundSizing" Value="InnerBorderEdge" />
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="Button">
                    <Grid x:Name="RootGrid"
                        Background="{TemplateBinding Background}"
                        Padding="{TemplateBinding Padding}"
                        BorderBrush="{TemplateBinding BorderBrush}"
                        BorderThickness="{TemplateBinding BorderThickness}"
                        contract7Present:CornerRadius="{TemplateBinding CornerRadius}"
                        contract7NotPresent:CornerRadius="{ThemeResource ControlCornerRadius}"
                        contract7Present:BackgroundSizing="{TemplateBinding BackgroundSizing}">

                        <VisualStateManager.VisualStateGroups>
                            <VisualStateGroup x:Name="CommonStates">
                                <VisualState x:Name="Normal"/>
                                <VisualState x:Name="PointerOver">
                                    <Storyboard>
                                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="Background">
                                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonBackgroundPointerOver}" />
                                        </ObjectAnimationUsingKeyFrames>
                                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="BorderBrush">
                                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonBorderBrushPointerOver}" />
                                        </ObjectAnimationUsingKeyFrames>
                                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="Foreground">
                                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonForegroundPointerOver}" />
                                        </ObjectAnimationUsingKeyFrames>
                                    </Storyboard>
                                    <VisualState.Setters>
                                        <Setter Target="AnimatedChevronIcon.(AnimatedIcon.State)" Value="PointerOver"/>
                                    </VisualState.Setters>
                                </VisualState>

                                <VisualState x:Name="Pressed">
                                    <Storyboard>
                                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="Background">
                                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonBackgroundPressed}" />
                                        </ObjectAnimationUsingKeyFrames>
                                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="BorderBrush">
                                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonBorderBrushPressed}" />
                                        </ObjectAnimationUsingKeyFrames>
                                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="Foreground">
                                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonForegroundPressed}" />
                                        </ObjectAnimationUsingKeyFrames>
                                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ChevronIcon" Storyboard.TargetProperty="Foreground">
                                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource DropDownButtonForegroundSecondaryPressed}" />
                                        </ObjectAnimationUsingKeyFrames>
                                    </Storyboard>
                                    <VisualState.Setters>
                                        <Setter Target="AnimatedChevronIcon.(AnimatedIcon.State)" Value="Pressed"/>
                                    </VisualState.Setters>
                                </VisualState>

                                <VisualState x:Name="Disabled">
                                    <Storyboard>
                                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="Background">
                                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonBackgroundDisabled}" />
                                        </ObjectAnimationUsingKeyFrames>
                                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="BorderBrush">
                                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonBorderBrushDisabled}" />
                                        </ObjectAnimationUsingKeyFrames>
                                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter" Storyboard.TargetProperty="Foreground">
                                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonForegroundDisabled}" />
                                        </ObjectAnimationUsingKeyFrames>
                                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ChevronIcon" Storyboard.TargetProperty="Foreground">
                                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonForegroundDisabled}" />
                                        </ObjectAnimationUsingKeyFrames>
                                    </Storyboard>
                                    <VisualState.Setters>
                                        <!-- DisabledVisual Should be handled by the control, not the animated icon. -->
                                        <Setter Target="AnimatedChevronIcon.(AnimatedIcon.State)" Value="Normal"/>
                                    </VisualState.Setters>
                                </VisualState>
                            </VisualStateGroup>
                        </VisualStateManager.VisualStateGroups>

                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="*"/>
                            <ColumnDefinition Width="80"/>
                        </Grid.ColumnDefinitions>

                        <ContentPresenter x:Name="ContentPresenter"
                            Content="{TemplateBinding Content}"
                            ContentTransitions="{TemplateBinding ContentTransitions}"
                            ContentTemplate="{TemplateBinding ContentTemplate}"
                            HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}"
                            VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"
                            AutomationProperties.AccessibilityView="Raw" />

                        <local:AnimatedIcon x:Name="AnimatedChevronIcon" Grid.Column="1" Margin="10,4,2,0" AutomationProperties.AccessibilityView="Raw"  local:AnimatedIcon.State="Normal">
                            <local:AnimatedIcon.Source>
                                <AnimatedVisuals:Controls_02_UpDown_Dropdown/>
                            </local:AnimatedIcon.Source>
                            <local:AnimatedIcon.FallbackIconSource>
                                <local:FontIconSource
                                    FontSize="8"
                                    FontFamily="{ThemeResource SymbolThemeFontFamily}"
                                    Foreground="{ThemeResource ButtonForegroundPressed}"
                                    Glyph="&#xE96E;"
                                    IsTextScaleFactorEnabled="False"/>
                            </local:AnimatedIcon.FallbackIconSource>
                        </local:AnimatedIcon>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>

    <Style TargetType="local:DropDownButton" BasedOn="{StaticResource DefaultDropDownButtonStyle}" />
```

### AnimatedIcon class member notes

| Name | Description | Default |
| :---| :---| :---|
| Source | Animation data, for example generated by the LottieGen tool. | null |
| FallbackSource | Gets or sets the static icon that will be used as a fallback when the OS cannot run the animated icon file.   | null |
| State | Attached property that the developer sets on AnimatedIcon. See below for more details. | null |
| Markers |	A dictionary (<string, double>) of all marker strings and their associated values found in the icon’s JSON file. The strings and values are based on what the designer names the markers in the Lottie After Effects file.  |	null |
| SetColorProperty | Takes a string and Windows.UI.Color value to set the color of a themed color property in the Lottie file.  |
| TryCreateAnimatedVisualSource | Method to get an IAnimatedVisual from an IAnimatedVisualSource2

When the developer sets the AnimatedIcon.State property on an element this is what happens: 
* The setter looks to see if the element that the property was just set on is an AnimatedIcon, if it is we finish. 
* If it is not, the setter checks to see if the first child of that element is an AnimatedIcon, if it is not, we finish. 
* If it is, the setter also sets the AnimatedIcon.State property on that child AnimatedIcon. 

## IAnimatedVisualSource2 interface

Provides a Composition `Visual` that can be animated.

`IAnimatedVisualSource2` is an `IAnimatedVisualSource`, so has the
[TryCreateAnimatedVisual](https://docs.microsoft.com/uwp/api/Microsoft.UI.Xaml.Controls.IAnimatedVisualSource.TryCreateAnimatedVisual),
which can be called to retrieve an
[IAnimatedVisual](https://docs.microsoft.com/uwp/api/Microsoft.UI.Xaml.Controls.IAnimatedVisual).
`IAnimatedVisual` can then be used to get a Composition
[Visual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual),
which can be added to a Xaml element tree using the methods of
[ElementCompositionPreview](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview).

`IAnimatedVisualSource2` also has a `SetColorProperty` method that can be used to set a color
of the animated visual, and a `Markers` property that can be used to find named positions
in the animation timeline.

`IAnimatedVisualSource2` does not derive from `IAnimatedVisualSource` because it does not have the diagnostics item. The developer does not need to have `IAnmiatedVisualSource` to use AnimatedIcon. 

```cpp
void AddVisualAndShowStartAnimation(
    const winrt::Border& element,
    const winrt::IAnimatedVisualSource2 source,
    const winrt::hstring& initialState,
    const winrt::hstring& steadyState,
    const winrt::Color& themeColor )
{
    winrt::IAnimatedVisual animatedVisual = source.TryCreateAnimatedIconVisual();

    // Find the positions in the animation of the two states
    auto const markers = source.Markers())
    auto const fromProgress = static_cast<float>(markers.Lookup(initialState));
    auto const toProgress = static_cast<float>(markers.Lookup(steadyState));

    // Set the theme color as the animated visual's foreground.
    source.SetColorProperty("Foreground", themeColor);

    // Helper that uses TryCreateAnimatedVisual to add the source to the XAML element tree.
    // See IAnimatedVisualSource.TryCreateAnimatedVisual for more information.
    AddVisualToElement(element, animatedVisual);

    // Helper to play an animation.
    // See IAnimatedVisual for more information.
    PlaySegment(animatedVisual.RootVisual(), fromProgress, toProgress);
}
```

## IAnimatedVisualSource2 interface member notes

| Name | Description |
| :-| :-|
| Markers | Provides a mapping from marker names to positions in the animation. The names are defined by the animation. |
| SetColorProperty | Sets a color for the animated visual. The property names are defined by the animation. |

# API Details

```c++
interface IAnimatedVisualSource2
{
    IAnimatedVisual TryCreateAnimatedVisual(Windows.UI.Composition.Compositor compositor);

    Windows.Foundation.Collections.IMapView<String, Double> Markers { get; };
    void SetColorProperty(String propertyName, Windows.UI.Color value);
};

unsealed runtimeclass AnimatedIcon : Windows.UI.Xaml.Controls.IconElement
 {
    AnimatedIcon();

    IAnimatedVisualSource2 Source{ get; set; };
    IconSource FallbackSource {get; set; };

    static Windows.UI.Xaml.DependencyProperty State{ get; };
    static void SetState(Windows.UI.Xaml.DependencyObject object, String value); 
    static String GetState(Windows.UI.Xaml.DependencyObject object); 

    static Windows.UI.Xaml.DependencyProperty SourceProperty{ get; };
}

unsealed runtimeclass AnimatedIconSource : IconSource
{
    AnimatedIconSource();

    IAnimatedVisualSource2 Source{ get; set; };
    IconSource FallbackIconSource{ get; set; };

    static Windows.UI.Xaml.DependencyProperty SourceProperty { get; };
    static Windows.UI.Xaml.DependencyProperty FallbackIconSourceProperty { get; };
}

```

# Appendix

## Fallback for older operating systems

AnimatedIcon will provide an API for a developer to add a static icon of type IconSource. This will be used when the operating system cannot play the animation from the LottieGen output. Lottie animations are not supported on operating systems older than RS5.


## Accessibility

This control is going to be part of the Icon family of controls so the accessibility requirements will reflect what the other icon controls have. AnimatedIcon will need to ensure that the high contrast brushes propagate correctly if the user changes their settings to a high contrast theme.  

If the user turns off animations in theri system settings, the AnimatedIcons will not animate. 