## Theming

By default Dashy comes with 20 built in themes, which can be applied from the dropwodwn menu in the UI

![Built-in Themes](https://i.ibb.co/GV3wRss/Dashy-Themes.png)

You can also add your own themes, apply custom CSS, and modify colors.

You can customize Dashy by writing your own CSS, which can be loaded either as an external stylesheet, set directly through the UI, or specified in the config file. Most styling options can be set through CSS variables, which are outlined below.

The following content requires that you have a basic understanding of CSS. If you're just beginning, you may find [this article](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps) helpful.


### How Theme-Switching Works

The theme switching is done by simply changing the `data-theme` attribute on the root DOM element, which can then be targeted by CSS. First off, in order for the theme to show up in the theme switcher, it needs to be added to the config file, under `appConfig.cssThemes`, either as a string, or an array of strings for multiple themes. For example:

```yaml
appConfig:
  cssThemes: ['tiger', 'another-theme']
```

You can now create a block to target you're theme with `html[data-theme='my-theme']{}` and set some styles. The easiest method is by setting CSS variables, but you can also directly override elements by their selector. As an example, see the [built-in CSS themes](https://github.com/Lissy93/dashy/blob/master/src/styles/color-themes.scss).

```css
html[data-theme='tiger'] {
  --primary: #f58233;
  --item-group-background: #0b1021;
}
```

Finally, from the UI use the theme dropdown menu to select your new theme, and your styles will be applied.

You can also set `appConfig.theme` to pre-select a default theme, which will be applied immediately after deployment.

### Setting Custom CSS

Custom CSS can be developed, tested and applied directly through the UI. Although you will need to make note of your changes to apply them across instances.

This can be done from the Config menu (spanner icon in the top-right), under the Custom Styles tab. This is then associated with `appConfig.customCss` in local storage. Any styles set this way can be synced across instances using the cloud backup and sync feature.

It's also possible to set CSS in the config file under `appConfig.customCss`. However this approach is not very neat, and if you do do it, first minify / compress your CSS and wrap in quotes, to ensure it does not cause any validation errors.

### Loading External Stylesheets

The URI of a stylesheet, either local or hosted on a remote CDN can be passed into the config file. The attribute `appConfig.externalStyleSheet` accepts either a string, or an array of strings. This is handled in [`ThemeHelper.js`](https://github.com/Lissy93/dashy/blob/master/src/utils/ThemeHelper.js).

For example:

```yaml
appConfig:
  externalStyleSheet: 'https://example.com/my-stylesheet.css'
```

```yaml
appConfig:
  externalStyleSheet: ['/themes/my-theme-1.css', '/themes/my-theme-2.css']
```
### Hard-Coding Section or Item Colors

Some UI components have a color option, that can be set in the config file, to force the color of a given item or section no matter what theme is selected. These colors should be expressed as hex codes (e.g. `#fff`) or HTML colors (e.g. `red`). The following attributes are supported:
- `section.color` - Custom color for a given section
- `item.color` - Font and icon color for a given item
- `item.backgroundColor` - Background color for a given icon

### CSS Variables

All colors as well as other variable values (such as borders, border-radius, shadows) are specified as CSS variables. This makes theming the application easy, as you only need to change a given color or value in one place. You can find all variables in [`color-palette.scss`](https://github.com/Lissy93/dashy/blob/master/src/styles/color-palette.scss) and the themes which make use of these color variables are specified in [`color-themes.scss`](https://github.com/Lissy93/dashy/blob/master/src/styles/color-themes.scss)

CSS variables are simple to use. You define them like: `--background: #fff;` and use them like: `body { background-color: var(--background); }`. For more information, see this guide on using [CSS Variables](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties).

You can determine the variable used by any given element, and visualize changes using the browser developer tools (Usually opened with `F12`, or Options --> More --> Developer Tools). Under the elements tab, click the Element Selector icon (usually top-left corner), you will then be able to select any DOM element on the page by hovering and clicking it. In the CSS panel you will see all styles assigned to that given element, including CSS variables. Click a variable to see it's parent value, and for color attributes, click the color square to modify the color. For more information, see this [getting started guide](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), and these articles on [selecting elements](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Select_an_element) and [inspecting and modifying colors](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Inspect_and_select_colors).

#### Top-Level Variables
These are all that are required to create a theme. All other variables inherit their values from these variables.

- `--primary` - Application primary color. Used for title, text, accents, and other features
- `--background` - Application background color
- `--background-darker` - Secondary background color (usually darker), used for navigation bar, section fill, footer etc
- `--curve-factor` - The border radius used globally throughout the application. Specified in `px`, defaults to `5px`
- `--dimming-factor` - Inactive elements have slight transparency. This can be between `0` (invisible) and `1` (normal), defaults to `0.7`


#### Targeted Color Variables
You can target specific elements on the UI with these variables. All are optional, since by default, they inherit their values from above

- `--heading-text-color` - Text color for web page heading and sub-heading. Defaults to `--primary`
- `--nav-link-text-color` - The text color for links displayed in the navigation bar. Defaults to `--primary`
- `--nav-link-background-color` - The background color for links displayed in the navigation bar
- `--nav-link-text-color-hover` - The text color when a navigation bar link is hovered over. Defaults to `--primary`
- `--nav-link-background-color-hover` - The background color for nav bar links when hovered over 
- `--nav-link-border-color` - The border color for nav bar links. Defaults to `transparent`
- `--nav-link-border-color-hover` - The border color for nav bar links when hovered over. Defaults to `--primary`
- `--search-container-background` - Background for the container containing the search bar. Defaults to `--background-darker`
- `--search-field-background` - Fill color for the search bar. Defaults to `--background`
- `--settings-background` - The background for the quick settings. Defaults to `--background`
- `--settings-text-color` - The text and icon color for quick settings. Defaults to `--primary`
- `--footer-text-color` - Color for text within the footer. Defaults to `--medium-grey`
- `--footer-text-color-link` - Color for any hyperlinks within the footer. Defaults to `--primary`
- `--item-text-color` - The text and icon color for items. Defaults to `--primary`
- `--item-group-outer-background` - The background color for the outer part of a section (including section head). Defaults to `--primary`
- `--item-group-background` - The background color for the inner part of item groups. Defaults to `#0b1021cc` (semi-transparent black)
- `--item-group-heading-text-color` - The text color for section headings. Defaults to `--item-group-background`;
- `--item-group-heading-text-color-hover` - The text color for section headings, when hovered. Defaults to `--background`
- `--config-code-background` - Background color for the JSON editor in the config menu. Defaults to `#fff` (white)
- `--config-code-color` - Text color for the non-highlighted code within the JSON editor. Defaults to `--background`
- `--config-settings-color` - The background for the config/ settings pop-up modal. Defaults to `--primary`
- `--config-settings-background` - The text color for text within the settings container. Defaults to `--background-darker`
- `--scroll-bar-color` - Color of the scroll bar thumb. Defaults to `--primary`
- `--scroll-bar-background` Color of the scroll bar blank space. Defaults to `--background-darker`
- `--toast-background` - Background color for the toast info popup. Defaults to `--primary`
- `--toast-color` - Text, icon and border color in the toast info popup. Defaults to `--background`
- `--welcome-popup-background` - Background for the info pop-up shown on first load. Defaults to `--background-darker`
- `--welcome-popup-text-color` - Text color for the welcome pop-up. Defaults to `--primary`

#### Non-Color Variables
- `--outline-color` - Used to outline focused or selected elements
- `--curve-factor-navbar` - The border radius of the navbar. Usually this is greater than `--curve-factor`
- `--scroll-bar-width` - Width of horizontal and vertical scroll bars. E.g. `8px`
- `--item-group-padding` - Inner padding of sections, determines the width of outline. E.g. `5px`
- `--item-shadow` - Shadow for items. E.g. `1px 1px 2px #130f23`
- `--item-hover-shadow` - Shadow for items when hovered over. E.g. `1px 2px 4px #373737`
- `--item-icon-transform` - A [transform](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) property, to modify item icons. E.g. `drop-shadow(2px 4px 6px var(--transparent-50)) saturate(0.65)`
- `--item-icon-transform-hover` - Same as above, but applied when an item is hovered over. E.g. `drop-shadow(4px 8px 3px var(--transparent-50)) saturate(2)`
- `--item-group-shadow` - The shadow for an item group/ section. Defaults to `--item-shadow`
- `--settings-container-shadow` - A shadow property for the settings container. E.g. `none`

#### Action Colors
These colors represent intent, and so are not often changed, but you can do so if you wish

- `--info` - Information color, usually blue / `#04e4f4`
- `--success` - Success color, usually green / `#20e253`
- `--warning` - Warning color, usually yellow / `#f6f000`
- `--danger` - Error/ danger color, usually red / `#f80363`
- `--neutral` - Neutral color, usually grey / `#272f4d`
- `--white` - Just white / `#fff`
- `--black` - Just black / `#000`


