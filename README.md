# Shopify Responsive Sticky Add-to-Cart (ATC) Section

This Shopify section provides a responsive "Sticky Add-to-Cart" bar that appears when the main product form (or a featured product's form) scrolls out of view. It aims to improve conversion rates by keeping the ATC functionality accessible.

## Features

*   **Context-Aware:** Works on standard product pages and can be configured for a specific "Featured Product" section (e.g., on the homepage).
*   **Responsive Design:** Adapts its layout for optimal viewing on desktop, tablet, and mobile devices.
*   **Dynamic Updates:**
    *   Product image, title, and price in the sticky bar update when a new variant is selected.
    *   The ATC button's state (e.g., "Add to cart" vs. "Sold out") updates with variant availability.
*   **Synchronization:**
    *   Changes to the variant selector on the main page are reflected in the sticky bar.
    *   Changes to the variant selector in the sticky bar are reflected on the main page.
    *   Quantity input is synchronized between the main page's form and the sticky bar's form.
*   **Customizable Appearance:** Control image visibility, size, title size, button style, and various price text styles through Shopify Theme Customizer settings.
*   **Flexible Trigger:** The sticky bar can be triggered based on the main ATC button scrolling out of view, with an optional offset.
*   **Fallback Selectors:** Includes basic fallback mechanisms if the primary CSS selectors for main page elements aren't found.

## How it Works

1.  **Initialization:**
    *   The section first determines if it's on a product page or configured for a featured product. It checks if the product is available and has variants.
    *   It identifies the main "Add to Cart" button and variant selector on the page using configurable CSS selectors. This is crucial for knowing when to show the sticky bar and for synchronizing selections.
2.  **Visibility Trigger:**
    *   The sticky bar is hidden by default.
    *   JavaScript monitors the scroll position. When the main "Add to Cart" button (or a defined offset) scrolls past the top of the viewport, the sticky bar becomes visible.
    *   It adds padding to the bottom of the `<html>` element to prevent content from being obscured by the sticky bar.
3.  **Dynamic Content & Synchronization:**
    *   When a variant is selected (either in the sticky bar or on the main page's product form), the sticky bar's content (image, price, button text/status) is updated.
    *   Quantity fields are kept in sync between the sticky bar and the main product form.
    *   The sticky bar uses its own `product-form` element to handle adding items to the cart.

## Installation

1.  **Create the Section File:**
    *   In your Shopify theme editor, go to `Sections`.
    *   Click "Add a new section".
    *   Name it something like `md-sticky-atc-responsive.liquid` (or any name you prefer).
    *   Remove any default code in the new file.
    *   Copy the entire provided code (Liquid, HTML, CSS, JavaScript, Schema) and paste it into this new section file.
    *   Save the file.
2.  **Add to Templates:**
    *   **For Product Pages:**
        *   Go to `Templates` and select your `product.json` template (or `product.liquid` for older themes).
        *   In the theme customizer interface, add the "Sticky ATC (Responsive)" section. It's generally best placed near the bottom of the sections list for the product page, as its DOM position doesn't affect its fixed screen position.
    *   **For Featured Product Sections (e.g., on Homepage):**
        *   If you want to use this for a featured product on the homepage or another page, add the "Sticky ATC (Responsive)" section to the relevant template (e.g., `index.json` for the homepage).
        *   You will then need to configure the "Context: Featured Product" settings within this section instance.

## Configuration Settings

These settings are available in the Shopify Theme Customizer when you select this section.

### General

*   **Enable "Sticky ATC"**: (`enable_section`)
    *   Quickly turn the entire sticky ATC feature on or off.
*   **Color Scheme**: (`color_scheme`)
    *   Select a color scheme defined in your theme's settings to apply to the sticky bar.
*   **Title Size (Base for Desktop)**: (`title_size`)
    *   Sets the base HTML heading tag (H1-H6) for the product title in the sticky bar. CSS will further adjust the font size for responsiveness.
*   **Show image**: (`show_img`)
    *   Toggle the visibility of the product image in the sticky bar.
*   **Image width (px)**: (`img_width`)
    *   Set the maximum width for the product image in the sticky bar (for desktop). It's scaled down on mobile.
*   **Image height (px)**: (`img_height`)
    *   Set the maximum height for the product image (used with `crop: 'center'`).
*   **Show quantity field**: (`show_quantity_input`)
    *   Toggle the visibility of the quantity input field in the sticky bar. If hidden, quantity defaults to 1.
*   **'Add to cart' button style**: (`btn_style`)
    *   Choose between "Primary" or "Secondary" button styles, assuming your theme defines these.

### Desktop Form Layout

These settings control the layout of the variant selector and quantity/button row specifically on larger screens (desktop).

*   **Variant Selector Width (Desktop)**: (`desktop_variant_select_width_mode`)
    *   Determines how the width of the variant dropdown is calculated on desktop.
        *   `Automatic (fits longest variant name)`: The dropdown will expand to fit the text of the longest variant option.
        *   `Fixed Width (truncate long names)`: The dropdown will have a specific width, and long variant names will be truncated with an ellipsis (...).
*   **Fixed Width for Variant Selector (Desktop)**: (`desktop_variant_select_fixed_width`)
    *   Value in pixels (e.g., `150px`).
    *   Only applies if "Variant Selector Width (Desktop)" is set to "Fixed Width". Defines the base width for the variant selector.

### Price Styling

Customize the appearance of the price elements within the sticky bar.

*   **Regular Price Font Size**: (`price_regular_font_size`)
    *   Relative size (e.g., `1em` is default, `1.2em` is 20% larger).
*   **Regular Price Color**: (`price_regular_color`)
    *   Color for the standard price. Defaults to transparent (inherits theme color).
*   **Sale Price Font Size**: (`price_sale_font_size`)
    *   Relative size for the sale price.
*   **Sale Price Color**: (`price_sale_color`)
    *   Color for the sale price. Defaults to red.
*   **Compare At Price Font Size**: (`price_compare_at_font_size`)
    *   Relative size for the original price (strikethrough) when an item is on sale.
*   **Compare At Price Color**: (`price_compare_at_color`)
    *   Color for the compare-at price. Defaults to transparent.

### Context: Product Page

These settings are relevant when the sticky ATC is used on a standard Shopify product page. The script automatically uses the current `product` object.

*   **Selector for Main ATC Button (Product Page)**: (`main_atc_button_selector_product_page`)
    *   **Crucial setting.** This is the CSS selector used to find the main "Add to Cart" button on the product page. The sticky bar appears when *this* button scrolls out of view.
    *   **Default:** `product-form:not(#md-sticky-atc .product-form) button[name="add"]`
        *   This targets a `button` with `name="add"` inside a `product-form` custom element, *excluding* the one within this sticky ATC itself (which has the ID `md-sticky-atc`).
    *   **How to find it:** Use your browser's developer tools (right-click > Inspect) on the main ATC button of your product page to find a unique and stable selector.
*   **Selector for Main Variant Dropdown (Product Page)**: (`main_variant_selector_product_page`)
    *   **Crucial for synchronization.** This CSS selector finds the main variant `<select>` dropdown on the product page.
    *   **Default:** `product-form:not(#md-sticky-atc .product-form) [name="id"]`
        *   Targets an element with `name="id"` (typically the variant select) inside a `product-form`, excluding the sticky ATC's own selector.
    *   **How to find it:** Inspect the main variant selector on your product page.

### Context: Featured Product (e.g., on Homepage)

Configure these if you're using the sticky ATC for a specific product featured outside of its dedicated product page (e.g., in a "Featured Product" section on the homepage).

*   **Enable for a Featured Product**: (`enable_on_featured_product`)
    *   Check this box if this instance of the sticky ATC is intended for a featured product.
*   **Select Product for Featured Display**: (`featured_product_object`)
    *   Use the product picker to choose which product this sticky ATC should represent. This is only used if "Enable for a Featured Product" is checked.
*   **Selector for Featured Product's ATC Button**: (`main_atc_button_selector_featured`)
    *   Similar to the product page version, but specific to the ATC button within your theme's "Featured Product" section.
    *   **Default:** `#shopify-section-featured-product .product-form__submit` (This is a common selector for Dawn theme's featured product section. **You will likely need to adjust this for your theme.**)
    *   **How to find it:** Add your theme's "Featured Product" section to a page, select a product, then inspect its "Add to Cart" button.
*   **Selector for Featured Product's Variant Dropdown**: (`main_variant_selector_featured`)
    *   CSS selector for the variant dropdown within your theme's "Featured Product" section.
    *   **Default:** `#shopify-section-featured-product [name="id"]` (Again, likely needs adjustment for your specific theme.)
    *   **How to find it:** Inspect the variant selector in your theme's "Featured Product" section.

### Advanced

*   **Offset**: (`offset`)
    *   A value in pixels. The sticky bar will appear when the user scrolls `offset` pixels past the bottom of the main ATC button (or past the top of the page if the main ATC button isn't found/configured).
    *   Can be used to trigger the bar slightly earlier or later, or as the primary trigger if the main ATC button selector is intentionally left blank or not found for a featured product.

### Spacing

*   **Padding top**: (`pt`)
    *   Top padding for the sticky bar content in `rem` units.
*   **Padding bottom**: (`pb`)
    *   Bottom padding for the sticky bar content in `rem` units.

## Important Notes & Troubleshooting

*   **CSS Selectors are Key:** The accuracy of the `main_atc_button_selector_...` and `main_variant_selector_...` settings is critical for the sticky bar to function correctly (both for visibility and synchronization). If the sticky bar doesn't appear or sync, these selectors are the first thing to check. Use browser developer tools meticulously.
*   **Theme Compatibility:** While designed to be broadly compatible, complex themes or themes with heavily customized product forms might require adjustments to the default CSS selectors or even minor CSS tweaks within this section for optimal integration.
*   **JavaScript:** The section relies heavily on JavaScript. Ensure JavaScript is enabled in the browser. It uses standard browser APIs and `Shopify.formatMoney` (which should be available in most themes).
*   **Quantity Input Synchronization:** The script looks for quantity inputs (`input[name="quantity"]`) in the main product form (excluding the sticky bar's own form). If your theme uses a different name or structure for quantity inputs, synchronization might fail. The current selector for the main quantity input is:
    `product-form:not(#md-sticky-atc .product-form) input[name="quantity"], form[action$="/cart/add"]:not(#product-form-{{ section.id }}) input[name="quantity"]`
*   **Testing:** Thoroughly test on different devices (desktop, tablet, mobile) and browsers after implementation and configuration. Test with products that have variants and products without variants (though this section is primarily designed for products *with* variants).

## Contributing

If you have improvements or bug fixes, feel free to fork the repository (if this were on GitHub) and submit a pull request.
