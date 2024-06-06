# [End-to-End Tests](https://developer.salesforce.com/docs/platform/lwc/guide/testing-dom-api.html)

> To find Lightning web components on the page, look for element names that contain a hyphen. Select the element and run $0.shadowRoot in the Console. A Lightning web component returns #document-fragment.

## [Working with Shadow DOM Elements using Webdriver](https://www.seleniumeasy.com/selenium-tutorials/accessing-shadow-dom-elements-with-webdriver)

Example:

We will look an example using Chrome's download page. If you observe the below image, to get the header text (h1 tag), we need to navigate to 3 nested shadow root elements.

![Chrome's download page](https://www.seleniumeasy.com/sites/default/files/pictures/selenium/shadow_dom_elements_with_selenium.jpg)

```
@Test
public void testGetText_FromShadowDOMElements() {
    System.out.println("Open Chrome downloads");
    driver.get("chrome://downloads/");

    System.out.println("Validate downloads page header text");
    WebElement root1 = driver.findElement(By.tagName("downloads-manager"));

            //Get shadow root element
    WebElement shadowRoot1 = expandRootElement(root1);

    WebElement root2 = shadowRoot1.findElement(By.cssSelector("downloads-toolbar"));
    WebElement shadowRoot2 = expandRootElement(root2);

    WebElement root3 = shadowRoot2.findElement(By.cssSelector("cr-toolbar"));
    WebElement shadowRoot3 = expandRootElement(root3);

    String actualHeading = shadowRoot3.findElement(By.cssSelector("div[id=leftContent]>h1")).getText();
    // Verify header title
    Assert.assertEquals("Downloads", actualHeading);

}
```

```
//Returns webelement
public WebElement expandRootElement(WebElement element) {
    WebElement ele = (WebElement) ((JavascriptExecutor) driver)
.executeScript("return arguments[0].shadowRoot",element);
    return ele;
}
```

## Side notes

### [CSS Stylesheets](https://developer.salesforce.com/docs/platform/lwc/guide/create-components-css.html)

- Look for the tag, try to use `:host` *pseudo-class* e.g. `c-child :host(.active)`.

### [Anti-Patterns for Component Styling](https://developer.salesforce.com/docs/platform/lwc/guide/create-components-css-antipatterns.html)

- Use `.highlight.yellow` instead `[class="highlight yellow"]`
- Don't rely on patterns like `lwc-2s44vctlls4-host` or `lwc-2s44vctlls4` to locate components.
- The [lwc-recipes](https://github.com/trailheadapps/lwc-recipes) repo has a [miscDomQuery](https://github.com/trailheadapps/lwc-recipes/tree/main/force-app/main/default/lwc/miscDomQuery) component that demonstrates DOM querying using querySelectorAll.

### [Access Elements the Component Owns](https://developer.salesforce.com/docs/platform/lwc/guide/create-components-dom-work.html)

- Would it be worth to look for a template tag?
- Would it be worth to look for `ref` attribute?
  ```
  <template>
    <div lwc:ref="myDiv"></div>
  </template>
  ```
  ```
  export default class extends LightningElement {
  renderedCallback() {
      console.log(this.refs.myDiv);
    }
  }
  ```

### [Style with lightning design system](https://developer.salesforce.com/docs/platform/lwc/guide/create-components-css-slds.html)

Many base components are built from SLDS component blueprints. A few examples include the `lightning-accordion`, `lightning-card`, and `lightning-tree` components.

Base components provide a class attribute so that you can add an SLDS utility class or custom class to the outer elements of the components. For example, to apply a margin utility class to the lightning-button base component, use class="slds-m-left_medium". Here it’s used to increase the margin between buttons.

```
<template>
  <lightning-button label="Submit" title="submit" onclick={handleSubmit}></lightning-button>
  <lightning-button
    variant="brand"
    label="Cancel"
    title="cancel"
    onclick={handleCancel}
    class="slds-m-left_medium"
  ></lightning-button>
</template>
```

To change the styling of a base Lightning component, first check the [Component Reference](https://developer.salesforce.com/docs/component-library/overview/components) to see whether the component has [design variations](https://developer.salesforce.com/docs/platform/lwc/guide/create-components-css-variants.html). To change the spacing of a component, such as the alignment, padding, margin, or even its typography, use [Lightning Design System utility classes](https://lightningdesignsystem.com/utilities/alignment/).

- Look for custom classes, i.e., don’t target `.slds-input`, as it’s part of the base component internal markup. Instead, look for a custom class inside the class attribute.

### [Shadow DOM](https://developer.salesforce.com/docs/platform/lwc/guide/create-dom.html)

To understand the [shadow tree](https://dom.spec.whatwg.org/#shadow-trees), let’s look at some markup. This markup contains two Lightning web components: c-todo-app and c-todo-item. The #shadow-root [document fragment](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment) defines the boundary between the DOM and the shadow tree. Elements below the shadow root are in the shadow tree.

```
<c-todo-app>
  #shadow-root
  <div>
    <p>Your To Do List</p>
  </div>
  <c-todo-item>
    #shadow-root
    <div>
      <p>Go to the store</p>
    </div>
  </c-todo-item>
</c-todo-app>
```

#### DOM APIs

> Don’t use these DOM APIs to reach into a component’s shadow tree in orgs that use Lightning Locker.

Lightning Locker prevents you from breaking shadow DOM encapsulation between Lightning web components by blocking these APIs. However, in Aura components version 39.0 and earlier, Lightning Locker is disabled at the component level, so an Aura component could have code that fails.

These APIs aren’t restricted by Lightning Web Security (LWS). LWS prevents you from breaking shadow DOM encapsulation by enforcing a value of closed on the ShadowRoot's mode property for all components.

### [Pass Markup into Slots](https://developer.salesforce.com/docs/platform/lwc/guide/create-components-slots.html)

#### Access Elements Passed Via Slots

The `<slot></slot>` element is part of a component's shadow tree. To access elements in its shadow tree, a component calls `this.template.querySelector()` and `this.template.querySelectorAll()`.

However, the DOM elements that are passed into the slot aren't part of the component's shadow tree. To access elements passed via slots, a component calls `this.querySelector()` and `this.querySelectorAll()`.