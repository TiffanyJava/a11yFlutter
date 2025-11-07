 Conversation ouverte. 1 message non lu.

Aller au contenu
Utiliser Gmail avec un lecteur d'écran
1 sur 6 146
a11yFlutter
Boîte de réception

Tiffany Voguin <voguin.tiffany@gmail.com>
16:20 (il y a 2 minutes)
À moi

---
description: "Guidance for creating more accessible code with Flutter"
applyTo:  "**/*.dart"
author : "Tiffany Voguin"
---

# Instructions for accessibility

Since the 28 june of 2025, the European Accessibility Act (EAA), requires full accessibility for numerical services (which refers to 100 % conformity to the AA level required by the mobile referentials (RAAM)). In order to meet the law and contribute to an inclusive world, you will generate code that is accessible to users with disabilities, including those who use assistive technologies such as screen readers, voice access, and even keyboard navigation (yes it does exist for mobile too) ! 

## General assesmments

1. Code must conform to [RAAM](https://accessibilite.public.lu/fr/raam1.1/referentiel-technique.html).
2. Go beyond minimal RAAM conformance wherever possible to provide a more inclusive experience.
3. Before generating code, reflect on these instructions for accessibility, and plan how to implement the code in a way that follows the instructions and is RAAM compliant.
4. After generating code, review it against RAAM and these instructions. Iterate on the code until it is accessible.
5. Finally, inform the user that it has generated the code with accessibility in mind, but that accessibility issues still likely exist and that the user should still review and manually test the code to ensure that it meets accessibility instructions.
6. You can use Accessibility Inspector for iOs or Accessibility Scanner for Android to check accessibility. 

## Bias Awareness - Inclusive Language

In addition to producing accessible code, GitHub Copilot and similar tools must also demonstrate respectful and bias-aware behavior in accessibility contexts. All generated output must follow these principles:

- **Respectful, Inclusive Language**
  Use people-first language when referring to disabilities or accessibility needs (e.g., “person using a screen reader,” not “blind user”). Avoid stereotypes or assumptions about ability, cognition, or experience.

- **Bias-Aware and Error-Resistant**
  Avoid generating content that reflects implicit bias or outdated patterns. Critically assess accessibility choices and flag uncertain implementations. Double check any deep bias in the training data and strive to mitigate its impact.

- **Verification-Oriented Responses**
  When suggesting accessibility implementations or decisions, include reasoning or references to standards (e.g., WCAG, platform guidelines). If uncertainty exists, the assistant should state this clearly.

- **Clarity Without Oversimplification**
  Provide concise but accurate explanations—avoid fluff, empty reassurance, or overconfidence when accessibility nuances are present.

- **Tone Matters**
  Copilot output must be neutral, helpful, and respectful. Avoid patronizing language, euphemisms, or casual phrasing that downplays the impact of poor accessibility.

## Persona based instructions

### Cognitive instructions

Disclaimer : the following list is not exhaustive but is the bare minimum. 

- Prefer plain language whenever possible ; 
- Ensure that navigation items are always displayed in the same order across the application ; 
- Keep the interface clean and simple - reduce unnecessary distractions ; 
- Animations should not last more than 5 seconds or should have controls ;  
- Flashing content should not last more than 3 seconds ; 
- The screen should adapt to portrait orientation and landscape orientation ; 
- The text should not be truncated.
  

### Keyboard instructions (yes, some people (especially people with visual impairment or motor impairment) are using the keyboard or contactor on their phones)

- All interactive elements need to be keyboard navigable and receive focus in a predictable order (usually following the reading order).
- Keyboard focus must be clearly visible at all times so users can visually determine which element has focus. WCAG requires a minimum contrast ratio of 3:1 between the focus indicator and its background.
- In Flutter, you can implement focus indicators using `WidgetStateProperty.resolveWith` to conditionally style widgets based on their state (e.g., `WidgetState.focused`).
- For buttons, create a `BorderSide` with a color that has at least a 3:1 contrast ratio with the background when the widget is in a focused state.
Example:

```dart
elevatedButtonTheme: ElevatedButtonThemeData(
  style: ElevatedButton.styleFrom(
    side: BorderSide(color: Colors.transparent),
  ).copyWith(
    side: WidgetStateProperty.resolveWith((states) {
      if (states.contains(WidgetState.focused)) {
        return BorderSide(color: Colors.black, width: 2.0);
      }
      return BorderSide(color: Colors.transparent);
    }),
  ),
),
```
- All interactive elements need to be keyboard operable. For example, users need to be able to activate buttons, links, and other controls. Users also need to be able to navigate within composite components such as menus, grids, and listboxes.
- Static (non-interactive) elements, should not be focusable.
- Hidden elements must not be keyboard focusable.
- Keyboard navigation inside components: some composite elements/components will contain interactive children that can be selected or activated. Examples of such composite components include grids (like date pickers), comboboxes, listboxes, menus, radio groups, tabs, toolbars, and tree grids. For such components:
  
  - When the container receives keyboard focus, the appropriate sub-element should show as focused. This behavior depends on context. For example:
    - If the user is expected to make a selection within the component (e.g., grid, combobox, or listbox), then the currently selected child should show as focused. Otherwise, if there is no currently selected child, then the first selectable child should get focus.
    - Otherwise, if the user has navigated to the component previously, then the previously focused child should receive keyboard focus. Otherwise, the first interactive child should receive focus.
- Keyboard focus must not become trapped without a way to escape the trap (e.g., by pressing the escape key to close a dialog).



#### Common keyboard commands:

- `Tab` = Move to the next interactive element.
- `Arrow` = Move between elements within a composite component, like a date picker, grid, combobox, listbox, etc.
- `Enter` = Activate the currently focused control (button, link, etc.)
- `Escape` = Close open open surfaces, such as dialogs, menus, listboxes, etc.



### Low vision instructions

- Prefer dark text on light backgrounds, or light text on dark backgrounds.
- Do not use light text on light backgrounds or dark text on dark backgrounds.
- The contrast of text against the background color must be at least 4.5:1. Large text, must be at least 3:1. All text must have sufficient contrast against it's background color.
  - Large text is defined as 18.5px and bold, or 24px.
  - If a background color is not set or is fully transparent, then the contrast ratio is calculated against the background color of the parent element.
- Parts of graphics required to understand the graphic must have at least a 3:1 contrast with adjacent colors.
- Parts of controls needed to identify the type of control must have at least a 3:1 contrast with adjacent colors.
- Parts of controls needed to identify the state of the control (pressed, focus, checked, etc.) must have at least a 3:1 contrast with adjacent colors.
- Color must not be used as the only way to convey information. E.g., a red border to convey an error state, color coding information, etc. Use text and/or shapes in addition to color to convey information.
Apple iOs has a system parameter which is : `Differentiate without color`. It is allowing blind color users to see an alternative of the color given image but you should note that : 
- On one hand, Android does not have a direct equivalent, which forces developers to consider platform-specific solutions, even though the very principle of accessibility aims for a unified and inclusive experience. 
- Beyond that, this also requires iOS developers as well as designers to plan for both versions in the code, since the `accessibilityDifferentiateWithoutColor` function takes as a parameter a boolean from the application that will be affected by this overlay.
- The content of the screen should be adapted with the size of the text x2. 

### Screen reader instructions

#### Images 

Images which are not conveying informations should be ignored. In Flutter, there is a class that allows you to do so. You can use ExcludeSemantics to exclude images semantic. 
However, you should notice that for the images, there is a slight difference between Android and iOs. Indeed, the image role is not rendered uniformily across all environments. 

Extract of the RAAM : 


- "TalkBack (Android): the nature of graphic elements is not rendered when they are integrated into a native application; the screen reader will render the description if defined but will not announce "image". However, for images integrated in web views, the screen reader renders "Graphic elements".
- VoiceOver (iOS): whether in a native application or in a web view, VoiceOver renders "Image" for images it can access.

Note from the website: In general, depending on the development method, it is also possible that the role is rendered neither on TalkBack nor on VoiceOver. Therefore, observation alone will determine the nature of the element. It is not essential that the "Image" role be rendered in most cases. Except in special cases where identification of the role is essential, the absence of a rendered role cannot constitute non-compliance." 

Source: RAAM 1.1 : Glossaire - Élément graphique


Example : 
##### Decorative image
``` dart 
 // Decorative image - excluded from screen readers
          ExcludeSemantics(
            child: Image.asset(
              'assets/decorative_pattern.png',
              width: 100,
              height: 100,
            ),
          ),
          
```
##### Image which conveys informations
``` dart 
  // Informative image - includes alt text for screen readers
          Semantics(
            label: 'Company',
            child: Image.asset(
              'assets/logo_company.png',
              width: 100,
              height: 100,
            ),
          ),
``` 


#### Interactive components

- All elements must correctly convey their semantics, such as name, role, value, states, and/or properties. Usually on Flutter, if you use material librabry, the native buttons are accessible but you might need to use custom components. 
- For instance, InkWell component is a non semantic widget element. It is allowing you to develop various custom clickable areas. To make it accessible, you should add a role to it and in order to do so, you should use the class Semantics of FLutter. Semantics is a class that is useful to turn your custom widgets into accessible custom widgets.

Example of a button with Inkwell: 

```dart
Semantics(
  button: true, // role
  label: 'Add to your favorites', //Accessible name
  child: InkWell(
    onTap: () {
      // Handle tap
    },
    child: Container(
      padding: EdgeInsets.all(16.0),
      child: Text('Add'), // Visible name 
    ),
  ),
)
``` 
Example of a link wih Inkwell : 

``` dart
Semantics(
  link: true, // role
  label: 'Add to your favorites', //Accessible name
  child: InkWell(
    onTap: () {
      // Handle tap
    },
    child: Container(
      padding: EdgeInsets.all(16.0),
      child: Text('Add'), // Visible name 
    ),
  ),
)
``` 
As aria technology with non semantic elements, we recreated the button's semantic with class Semantics. 

#### Alerts

Each element that is appearing on the screen like a message status must be announced by screen readers. To do so, you should use a method from the class Semantics which is service.announce(). It is useful for notifications, message status, alerts ...  
Despite the fact that this method is quite simple to use, you should be careful on iOs. Indeed, iOs has an alert system which is in conflict with this method. It will cut himself and not announce the message in Semantics service.announce(). You can come accross this difficulty by adding a delay targetting iOs. 

Example : 

``` dart 
void notifyListener(
  String message, {
  TextDirection textDirection = TextDirection.ltr, // permit to choose the reading direction (if you have to developp apps in arabic, le value is going to be rtl)
}) async {
  if (Platform.isIOS) {
    // Delay only for iOS
    await Future.delayed(Duration(milliseconds: 500));
  }
  
  SemanticsService.announce(
    message,
    textDirection,
  );
}

void messageStatut() {

  // Announce notification to screen readers
  notifyListener('The message has been sent');
}

void onError() {
  notifyListener('An error occurred, please try again');
}

void onItemAdded() {
  notifyListener('Item added to cart');
}
```

For content that changes dynamically and needs to be announced automatically (like live scores, timers, progress indicators, or status updates), use live regions with the `liveRegion` property.

Live regions automatically announce changes to screen readers without requiring explicit calls to `SemanticsService.announce()`.



Example with a loading indicator :

```dart
Semantics(
  liveRegion: true,
  label: isLoading ? 'Loading content, please wait' : 'Content loaded',
  child: isLoading 
    ? CircularProgressIndicator()
    : ContentWidget(),
)
```


#### Is each state of a toggle control presented to the user perceivable ?

All states of toggle controls (such as switches, checkboxes, or toggle buttons) must be clearly perceivable to all users, including those using assistive technologies. This means:

- Visual indicators must clearly distinguish between different states (e.g., on/off, checked/unchecked, selected/unselected) ; 
- State changes must be announced to screen readers using appropriate semantic properties ; 
- Color alone should not be the only way to convey state changes ; 
- Sufficient contrast ratios must be maintained for state indicators. 


Note : checkboxes do not exist in iOs operating system. Prefer using switch which are better supported by mobiles. 

#### Information structure 

In every screen, is the information structured with headings ? 

- Use headings to introduce new sections of content. Headings permit to screen reader's users to navigate faster throughout the screen. Use class Semantic to wrapp your text and add the attribute bool header : true. In comparison to the web which has heading hierarchy, mobile operating systems don't permit it so it is only existing, title or not title. 

Exemple : 

```dart
Semantics(
  header: true,
  child: Text(
    'Section Title',
    style: TextStyle(
      fontSize: 24,
      fontWeight: FontWeight.bold,
    ),
  ),
)
```

#### Is each list perceivable with screen readers ? 

In comparison to the web, mobile's operating systems do not support semantic lists. So, it is only required to add the term :" List" before list that can be visually identified as a list. 
In order to validate the criterion List, the best is to add a visually hidden text and write :"List of X elements".

Note : you can hide text and make them only accessible for screen readers with Visibility following this parameters : 
``` dart 
Visibility(
    visible: false,
    maintainSemantics: true,
    maintainSize: true,
    maintainAnimation: true,
    maintainState: true,
    child: Text('Hidden label for screen readers'),
                  ),
```
 Example : 
``` dart

Container(
  height: 300,
  margin: const EdgeInsets.only(top: 24),
  decoration: BoxDecoration(
    border: Border.all(color: Colors.grey),
    borderRadius: BorderRadius.circular(8),
  ),
  child: Column(
    children: [
    // Wrap the visual list so the label describes the list itself. When the
    // list receives accessibility focus screen readers (at least for talkback) should announce the label.
    Expanded(
      child: Semantics(
        container: true,
        label: 'List of 2 elements',
        explicitChildNodes: true, //add this when you want the parent label to be announced first, otherwise Flutter may merge the children and the parents which can cause the text to be skipped or combinated. 
        child: ListView( // Make visual lists in Flutter but does not have any semantic
          children: [
            ListTile(
              leading: Icon(Icons.person),
              title: Text('Item 1'),
              subtitle: Text('Description 1'),
            ),
            ListTile(
              leading: Icon(Icons.email),
              title: Text('Item 2'),
              subtitle: Text('Description 2'),
            ),
          ],
        ),
      ),
    ),
  ], ),
  ),    
    
```
### Voice Access instructions

- The accessible name of all interactive elements must contain the visual label in the beginning or the end of the accessible name. This is so that voice access users can issue commands like "Click \<label>". If an `SemanticLabel` attribute is used for a control, then it must contain the text of the visual label.
- Interactive elements must have appropriate roles and keyboard behaviors.


### In each screen, is the visible content that conveys information accessible to assistive technologies ?

## Grouping Semantic Elements

> **Note (RAAM):** In applications, it is possible to group elements together. For example, in a product catalog, each item has a title, a price, and a description. Instead of requiring the screen reader to focus on each of the 3 elements individually, the application can be designed so that the screen reader accesses only the item as a whole. This way, the screen reader conveys all the information without the user having to perform multiple gestures to reach the 3 elements. This is compliant (and even encouraged since it reduces the actions required to browse the content), but you must ensure that all contained text is properly conveyed by the screen reader.
> 
> *Source: Référentiel d'Audit d'Accessibilité Mobile (RAAM)*

In Flutter, implement this using `MergeSemantics`:


``` dart
// Without MergeSemantics - text is announced separatly
ListTile(
  leading: Icon(Icons.person),
  title: Text('John Doe'),
  subtitle: Text('Software Engineer'),
  trailing: Icon(Icons.chevron_right),
)
// The screen reader will announce : "person icon", break "John Doe", break "Software Engineer", break "chevron right icon"

// With MergeSemantics - Text is packed in one announce
MergeSemantics(
  child: ListTile(
    leading: Icon(Icons.person),
    title: Text('John Doe'),
    subtitle: Text('Software Engineer'),
    trailing: Icon(Icons.chevron_right),
  ),
)
// The screen reader will announce : "John Doe, Software Engineer"
```

### Forms instructions

- Each field should have a visible label ; 
- The label should be read when the focus of the screen reader is on it ; 
In Flutter, the native textfields are accessible. 
- The label should inform the user of the purpose of the form field ; 
- Each label should be next to the form field ; 


### Grouping Related Form Fields 

When form fields share the same nature or belong to the same logical group, they must meet the following conditions if necessary:

Disclaimer : The semantic "group" doesn't exist in Flutter or in general in mobile OS. So the following code is compliant to the referential but not as good as the web. 

- Fields must be grouped within a common element
- The grouping must have a relevant title/label


The group title must clearly describe the nature of the fields it contains, helping users understand the context and relationship between the fields.

### Implementation in Flutter

Use `Semantics` and place the word 'groupe' in the label :
```dart
Column(
  
    // Group header - visually hidden but accessible
    Visibility(
        visible: false,
        maintainSemantics: true,
        maintainSize: true,
        maintainAnimation: true,
        maintainState: true,
        child: Text('Adress informations group'),
                  ),
    ),
    
    // Grouped fields
    Column(
      children: [
        TextField(
          decoration: InputDecoration(
            label : 'Street Address',
          ),
        ),
        SizedBox(height: 16),
        TextField(
          decoration: InputDecoration(
            label : 'City',
          ),
        ),
        SizedBox(height: 16),
        TextField(
          decoration: InputDecoration(
            label: 'Postal Code',
          ),
        ),
        SizedBox(height: 16),
        TextField(
          decoration: InputDecoration(
            label: 'Country',
          ),
        ),
      ],
    ),


```


### Examples of Related Fields with format and text error

**email:**
``` dart

Text('The form field marked with an asterisk * are mandatory' )

TextField(
  keyboardType: TextInputType.emailAddress, // Display the keyboard to enter a mail address. 
  decoration: InputDecoration(
    label : 'Email (@mail.com) *',
    errorText: emailError ? 'Invalid mail adress format. Example : toto@mail.com : null,'
  ),
)

TextField(
  keyboardType:TextInputType.number, // Display the keyboard to enter a number. 
  decoration: InputDecoration(
    label: 'Date (MM/DD/AAAA)',
    errorText: dateError ? 'Invalid date format. Expected format MM/DD/AAAA. Example : 04/07/2000 : null',
  ),
)
```
- TextInputType.text : default keyboard for text ; 
- TextInputType.number : for numbers ; 
- TextInputType.emailAdress ; 
- TextInputType.name ; 
- TextInputTpe.multiline  ;
- TextInputType.datetime ;
- TextInputType.phone ;
- TextInputType.streetAdress ;
- TextInputType.twitter ;
- TextInputType.url ;
- TextInputType.values ;
- TextInputType.visiblePassword ;



- The focus should go on the first form field that has an error ; 
- You can add autocomplete with TextField(
  autofillHints: [AutofillHints.email], // 
)


### Tap zone's size

The clickable zones should be 24px on 24px (AA) minimum but if possible, prefer 44px on 44px (AAA). 



