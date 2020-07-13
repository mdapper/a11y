# Web Accessibility

This repo contains information about web accessibility that I learned.

## Perceivable

Web content is made available to the senses - sight, hearing, and/or touch.

### Guideline 1.1

Text Alternatives: Provide text alternatives for any non-text content

**1.1.1 Non-text Content:**

- [ ] Images, form image buttons, and image map hot spots have appropriate, equivalent alternative text.
- [ ] Images that do not convey content, are decorative, or contain content that is already conveyed in text are given null alt text (alt="") or implemented as CSS backgrounds. All linked images have descriptive alternative text.
- [ ] Equivalent alternatives to complex images are provided in context or on a separate linked page.
- [ ] Form buttons have a descriptive value.
- [ ] Form inputs have associated text labels.
- [ ] Embedded multimedia is identified via accessible text.
- [ ] Frames and iframes are appropriately titled.

**1.3.2 Meaningful Sequence:**

- [ ] The reading and navigation order (determined by code order) is logical and intuitive.

If you change the order that elements are displayed this will not change tab order, they respect the order they appear in the DOM. Be careful when you do that.

A good habit is to navigate using tab on your product from time to time.

_Fixing:_

- The first thing you can try is to put them on the DOM at the same order they appear in the page.
- Also try to use the correct HTML elements to preserver the semantic
- If that is not possible you can use `tabindex`
- `tabindex="-1"` removes from the natural tab order, but can be focused using `focus()`. Useful for off screen elements like modals.
- `tabindex="0"` adds the element to the natural tab order
- `tabindex="5"` jumps to the front of the tab order (Anti-pattern!), the best way is to move it on the DOM

## Operable

Interface forms, controls, and navigation are operable.

### Focus - (Guideline 2.1)

Keyboard Accessible: Make all functionality available from a keyboard

**2.1.1 Keyboard:**

- [ ] All page functionality is available using the keyboard, unless the functionality cannot be accomplished in any known way using a keyboard (e.g., free hand drawing).
- [ ] Page-specified shortcut keys and accesskeys (accesskey should typically be avoided) do not conflict with existing browser and screen reader shortcuts.

Focus styles should be clear.

**Tab Order**

It's the order that we navigate with `tab`.

Some elements have are automatically inserted in the tab order. (inputs, buttons, select, etc).

Elements like `p`, `h1`, `h2`, `img` etc, are not focusable by default. In general there are no need to focus on elements that the user don't need to interact.

**2.1.2 No Keyboard Trap:**

[ ] Keyboard focus is never locked or trapped at one particular page element. The user can navigate to and from all navigable page elements using only a keyboard.

> Note: Only put the focus on elements that the user will interact

**Managing focus**

When a use click on something and the pages jump, or go to other route in a SPA, you should move the focus to some header. So if the user click tab they will be moved to the expected next element.

- Give an element, maybe a heading, the `tabindex="-1"` and call `focus()` on it
- You also can disable the focus styles for that element if it is not interactive

### Main content

Insert some skip links to navigate to the main content or other important stuff.

For that we can add an hidden link that allow to jump to the content. They are usually referred as _Skip Links_.

```html
<a href="#maincontent" class="skip-link">Skip to main content</a>
<nav>
  ...
</nav>
<main id="maincontent" tabindex="-1">
  ...
</main>
```

Styles:

```css
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #bf1722;
  color: #ffffff;
  padding: 8px;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}
```

### Creating complex components

The ARIA Authoring Practices doc (or "ARIA Design Patterns doc") is a great resource for figuring out what kind of keyboard support your complex components should implement.

**Roving focus:**

```html
<li tabindex="0" checked></li>
<li tabindex="-1"></li>
<li tabindex="-1"></li>
<li tabindex="-1"></li>
<li tabindex="-1"></li>
```

When you move to the next element using down arrow, change the first to `tabindex="-1"`, the one we selected to `tabindex="0"` and call `focus()` on it.

```html
<li tabindex="-1"></li>
<li tabindex="0" checked></li>
<li tabindex="-1"></li>
<li tabindex="-1"></li>
<li tabindex="-1"></li>
```

After the user go to the whole list we move it back to the first.

### Offscreen Content

## Understandable

Information and the operation of user interface must be understandable.

## Robust

Content must be robust enough that it can be interpreted by a wide variety of user agents, including assistive technologies.

## Resources

- [WCAG Guidelines](https://www.w3.org/TR/WCAG20/)
- [WCAG 2 Checklist](https://webaim.org/standards/wcag/checklist)
- [HTML Living Standard](https://html.spec.whatwg.org/multipage/interaction.html#attr-tabindex)
- ["Skip Navigation" Links](https://webaim.org/techniques/skipnav/)
- [WAI-ARIA Authoring Practices](https://www.w3.org/TR/wai-aria-practices/)
