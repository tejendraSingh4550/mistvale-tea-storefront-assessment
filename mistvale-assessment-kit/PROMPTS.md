# AI Prompt Log

## Prompt 1
- Tool: Codex
- Type: code
- prompt:

You are helping me complete the MeroxIO Developer Assessment for the Mistvale Tea Co. online store.

Before making any changes, inspect and understand the entire project.

Read and analyze:
- index.html
- BRAND.md
- all files inside the images folder

Do NOT modify, create, delete, rename, or overwrite any files yet.

First, understand the existing implementation, including:
- HTML structure
- CSS and visual design
- JavaScript functionality
- PRODUCTS data
- API object and how it is used
- search, category filter and sorting
- quick view
- cart and quantity handling
- coupon handling
- shipping/free-shipping logic
- delivery/pincode check
- newsletter
- countdown
- checkout form
- current images

Also compare the existing implementation against the assessment requirements and business rules in the README/context available in this project.

Create a detailed analysis only. Identify:
1. Existing bugs and suspicious logic.
2. Business-rule violations.
3. Design and UX problems.
4. Accessibility problems.
5. Responsive/mobile issues.
6. SEO and performance issues.
7. Important constraints that must be preserved.
8. A prioritized implementation plan for approximately 4 hours.

Important constraints:
- Keep the project as one HTML file.
- Keep all CSS and JavaScript inside index.html.
- Do not add frameworks or libraries.
- Do not change product IDs, names, or prices in PRODUCTS.
- Do not modify the API object between API START and API END.
- Do not change the approved legal text in the footer.
- Do not invent company facts, reviews, ratings, awards, or unsupported claims.
- Preserve the checkout form contract.

For this step, analysis only. Do not edit any files.

## Prompt 2
- Tool: Codex
- Type: code
- prompt:

Now start implementing the assessment, but work only on the commerce/cart logic first.

Before editing, re-read the relevant parts of index.html and preserve all assessment constraints.

Fix and test these areas:

1. Cart initialization:
- Safely handle a first visit when localStorage has no cart.
- Safely handle invalid/corrupted stored cart JSON.
- Always maintain a valid cart array.

2. Product pricing and stock:
- Treat PRODUCTS as the single source of truth for id, name, price and stock.
- Never calculate product prices from rendered page text.
- Do not change any product id, name or price.
- Do not allow sold-out products to be added.

3. Quantity rules:
- A shopper can buy at most 5 units of any product.
- Never allow quantity above the product's stock.
- Never allow quantity below 1.
- Prevent duplicate cart lines for the same product; increase the existing quantity instead.
- Cart count should represent total units, not number of cart lines.

4. Cart controls:
- Fix plus/minus quantity behavior.
- Fix quantity input validation.
- Fix removeItem so it removes only the selected cart item.
- Prevent duplicate event listeners or repeated actions after re-rendering.

5. Coupon WELCOME10:
- Case-insensitive.
- Only one coupon per order.
- Applying it multiple times must not increase the discount.
- Cart subtotal must be at least ₹399 before the coupon can apply.
- Give 10% off eligible items only.
- Gift products (category "Gifts") are not eligible.
- Maximum discount is ₹150.
- Recalculate the discount whenever cart contents change.
- Show clear user feedback for invalid or ineligible coupon attempts.

6. Shipping and totals:
- Shipping is ₹49 unless the amount after the discount reaches the free-shipping threshold.
- Do not silently choose a numeric free-shipping threshold because the README/page currently contains a contradiction.
- Keep the threshold decision isolated in one clearly identifiable place so it can be changed later.
- Use a central calculation function for subtotal, discount, shipping and final total.
- Round only the final total to the nearest rupee.
- Display all money values using ₹ and Indian digit grouping.

7. Sold-out sorting:
- Sold-out products must always appear last in every sort order.

Important:
- Do not modify the API object between API START and API END.
- Do not modify the approved legal text in the footer.
- Do not add frameworks or libraries.
- Keep all CSS and JavaScript inside index.html.
- Do not modify PRODUCTS ids, names or prices.
- Do not make unrelated design changes yet.
- Do not implement bonus features yet.

After making the changes:
1. Explain exactly what you changed.
2. List any assumptions you had to make.
3. List any potential remaining issues.
4. Tell me exactly how I should manually test the cart, quantity, coupon and shipping behavior.
5. Do not claim something works unless you verified it from the code or by running a test.

Keep the changes focused on commerce/cart logic only.

outcome: Accepted

## Prompt 3

- Tool: Codex
- Type: code
- Prompt:

Now continue with the next focused implementation step: fix the product discovery and quick-view functionality.

Do not redesign the page yet. Keep the current visual design unchanged as much as reasonably possible.

Fix these areas:

1. SEARCH
- Search must match the products correctly.
- The current search implementation has an indexOf() truthiness bug. Fix it.
- Results must always match exactly what is currently typed in the search box.
- Handle empty search correctly.
- Prevent an older/slower API response from overwriting a newer search result.
- Show a useful empty-results state when nothing matches.

2. CATEGORY FILTER
- Category filtering must work correctly.
- The "All" category must show all matching products instead of filtering for category "all".
- Category filtering must preserve the current search query.
- Changing category must not reset the selected sort.

3. SORTING
- Sorting must work together with search and category filtering.
- Do not mutate shared source/product arrays in a way that causes later operations to behave incorrectly.
- Use proper numeric comparisons for prices and other numeric sort values.
- Sold-out products must always remain last regardless of the selected sort order.
- Preserve the current search query and category when changing sort.

4. COMBINED DISCOVERY PIPELINE
Use one clear filtering/sorting pipeline:
PRODUCTS → search → category → sort → render

The currently selected search, category, and sort state should always be respected together.

5. QUICK VIEW
- Fix the existing quick-view bug caused by the loop variable/closure.
- Clicking a product's quick-view/image should open the correct product every time.
- The selected product's name, image, description and price must come from PRODUCTS.
- Do not read product prices from rendered text.
- Make sure the quick-view Add to Cart action adds the correct product.
- If the current size selector is not actually supported by the product data/backend, do not pretend it changes the product price or cart data. Either make it clearly informational or remove the misleading behavior.
- Do not invent product information.

6. EMPTY AND LOADING STATES
- Make search/filter operations understandable to the user.
- Do not leave stale results visible when the current query should produce different results.
- Provide a reasonable empty-results message.

Constraints:
- Do not modify the API object between API START and API END.
- Do not modify any PRODUCTS id, name, or price.
- Do not change the approved legal footer text.
- Keep all CSS and JavaScript inside index.html.
- Do not add frameworks or libraries.
- Do not redesign the page yet.
- Do not implement bonus features yet.

After making the changes:
1. Explain exactly what you changed.
2. Tell me which existing bugs were fixed.
3. Tell me how you verified search, category, sort and quick view.
4. List anything that still needs manual browser testing.
5. Do not claim a behavior works unless you actually verified it.

## Prompt 4

- Tool: Codex
- Type: code
- Prompt:

Now do a focused bug-fix pass for the current rendered storefront. Do not redesign the website yet.

Fix these visible issues:

1. CHARACTER ENCODING
- The browser currently displays broken characters such as:
  "â‚¹" instead of "₹"
  and "â˜…" instead of "★".
- Fix the document character encoding so all intended Unicode characters render correctly.
- Ensure the ₹ currency symbol, star ratings, hearts/icons, arrows, and other intended Unicode characters display correctly throughout the page.
- Do not replace the intended symbols with unrelated text.
- Do not change product IDs, names, prices, API data, or legal footer text.

2. SALE COUNTDOWN
- The current Diwali sale countdown displays negative values such as "-335d".
- Fix the countdown so it never displays negative days/hours/minutes/seconds.
- If the sale end date has already passed, show a sensible expired/ended state instead of negative numbers.
- Keep the existing sale messaging unless changing it is necessary to make the state truthful.
- Do not invent a new sale date.

3. PRODUCT CARD RENDERING
- Keep the current product data and functionality unchanged.
- Make sure product ratings and prices render correctly after the encoding fix.
- Do not change product IDs, names, descriptions, prices, stock, or categories.

4. BASIC HTML QUALITY
- Ensure the document has a correct UTF-8 character encoding declaration in the <head>.
- Check for any obvious malformed HTML that could cause the visible encoding/rendering problems.
- Do not add frameworks or external libraries.

CONSTRAINTS:
- Do not redesign the page.
- Do not replace the current product images yet.
- Do not change the API block between API START and API END.
- Do not modify PRODUCTS IDs, names, prices, or other business data.
- Do not modify the approved legal footer paragraph.
- Keep CSS and JavaScript inside index.html.
- Keep the changes focused only on the issues listed above.

After implementation:
1. Explain exactly what you changed.
2. Explain why the encoding issue occurred.
3. Explain how the countdown behaves before and after the sale end date.
4. Run a syntax/basic verification.
5. Tell me what I must manually check in the browser.

outcome: Accepted

## Prompt 5

- Tool: Codex
- Type: code
- Prompt:

Now move to the visual redesign phase of the MistVale Tea storefront.

The core commerce, search, filtering, sorting, and quick-view functionality has already been implemented and manually tested. Preserve that functionality.

Redesign the storefront to feel like a premium Indian tea brand: earthy, elegant, warm, editorial, and minimal rather than loud or playful.

IMPORTANT:
Do not break or rewrite the existing business logic.

VISUAL DIRECTION

1. OVERALL STYLE
- Create a premium tea-commerce aesthetic inspired by Indian tea estates and misty mountain landscapes.
- Use an earthy palette such as deep forest green, tea-leaf green, warm cream, muted beige, dark brown, and restrained gold/terracotta accents.
- Remove the current overly neon/pink/green appearance.
- Avoid excessive gradients, glowing effects, cartoon styling, or overly decorative elements.
- Use generous whitespace and clean visual hierarchy.
- Keep the design sophisticated and believable as a real premium tea brand.

2. HEADER / NAVIGATION
- Redesign the header to feel clean and premium.
- Keep the existing navigation functionality.
- Keep Home, Shop, About, Contact, search, and cart functionality.
- Make the header responsive.
- Improve spacing, typography, alignment, and contrast.
- Keep the cart count visible and readable.

3. HERO SECTION
- Redesign the hero section as a premium tea-brand hero.
- Use a calm misty tea-estate / Himalayan tea-garden visual direction.
- Keep the main brand message clear and readable.
- Keep the primary Shop CTA.
- Avoid huge text that overwhelms the viewport.
- Create a strong editorial composition with clear hierarchy.

4. PRODUCT SECTION
- Make the "Our Teas" section look like a premium product catalogue.
- Improve category filter buttons and sorting controls.
- Keep the existing search/filter/sort functionality.
- Product cards should have:
  - clean image area
  - product name
  - rating
  - short description
  - price
  - clear Add to Cart / Quick View interaction
  - stock/sold-out state
- Use consistent card dimensions and spacing.
- Reduce unnecessary decorative borders.
- Make cards feel premium rather than like basic demo cards.

5. PRODUCT IMAGES
- Keep the current product image paths/data working for now.
- Do not break image references.
- Do not modify PRODUCTS IDs, names, prices, stock, categories, or descriptions.
- Do not invent product data.
- We will replace placeholder product artwork with proper tea-product imagery in a later step.

6. SALE BANNER
- Keep the existing sale messaging and working countdown/ended state.
- Restyle the sale strip so it fits the premium MistVale visual system instead of using a loud neon color.
- Do not introduce negative countdown values again.

7. DELIVERY / PINCODE SECTION
- Improve the visual presentation of the delivery checker.
- Keep its existing functionality.
- Make the input and button accessible, clean, and consistent with the new design.

8. REVIEWS / TESTIMONIALS
- Redesign the testimonial section to feel premium and trustworthy.
- Improve typography, spacing, cards, and hierarchy.
- Keep the existing testimonial content unless there is a functional reason not to.

9. FOOTER
- Create a clean premium footer.
- Preserve the approved legal paragraph EXACTLY.
- Do not rewrite or shorten the approved legal text.
- Keep existing contact/social information.

10. RESPONSIVE DESIGN
- Make the layout work properly on:
  - desktop
  - tablet
  - mobile
- Product cards should become fewer columns on smaller screens.
- Navigation should remain usable on mobile.
- Search, category controls, sorting, cart and quick view must remain usable on touch devices.
- Avoid horizontal scrolling.

11. TYPOGRAPHY
- Use a refined typography hierarchy.
- Headings can have an elegant editorial/serif character.
- Body text must remain highly readable.
- Avoid using decorative fonts for large amounts of body text.
- Maintain strong contrast.

CONSTRAINTS

- Do not modify the API object between API START and API END.
- Do not modify PRODUCTS IDs, names, prices, stock, categories, or descriptions.
- Do not break existing cart logic.
- Do not break search, category filtering, sorting, or quick view.
- Do not change the approved legal footer paragraph.
- Keep CSS and JavaScript inside index.html.
- Do not add frameworks or external libraries.
- Do not remove required functionality.
- Do not implement unrelated bonus features.
- Do not replace product images yet.

After implementation:

1. Explain the visual changes made.
2. Explain how you preserved existing functionality.
3. Run a syntax/basic verification.
4. Check that the existing cart, search, filter, sort and quick-view code remains intact.
5. Tell me exactly what needs to be manually checked in the browser.
6. Do not claim responsive behavior works unless you actually verified it.

outcome: Accepted
 Why: Redesigning the MistVale storefront into a premium, earthy, editorial tea-brand experience while preserving all existing functionality.

 ## Prompt 6

- Tool: Codex
- Type: code
- Prompt:

Now perform the accessibility, SEO, performance, and responsive-polish phase.

Do not change the established visual design unnecessarily. Do not change product/business data or core commerce behavior.

1. ACCESSIBILITY
- Ensure the page has a meaningful document title and language attribute.
- Ensure the main page structure uses appropriate semantic HTML landmarks.
- Check heading hierarchy and avoid unnecessary heading-level jumps.
- Ensure all meaningful images have useful alt text.
- Decorative images should use appropriate empty alt text where applicable.
- Ensure buttons have accessible names.
- Ensure icon-only controls have accessible labels.
- Ensure form inputs have associated labels or appropriate accessible names.
- Ensure search, category controls, sorting, cart controls, quick view, newsletter, and delivery checker are keyboard accessible.
- Add visible :focus-visible states that fit the existing design.
- Ensure keyboard users can operate important interactive elements without relying on hover.
- Check color contrast for text, buttons, links, and important UI states.
- Ensure sold-out and error states are not communicated by color alone.

2. RESPONSIVE / MOBILE POLISH
- Review the existing responsive CSS carefully.
- Prevent horizontal overflow at narrow widths.
- Ensure product cards, buttons, forms, navigation, hero content, cart drawer, quick view, newsletter popup, and sale strip fit within the viewport.
- Ensure touch targets are reasonably sized.
- Preserve the existing responsive design rather than replacing it with a completely different layout.

3. SEO
- Add or improve a meaningful meta description.
- Add appropriate viewport metadata.
- Ensure the page has one clear primary H1.
- Add useful Open Graph metadata where appropriate without inventing unsupported claims.
- Add JSON-LD structured data only for information that is actually supported by the existing project.
- Include appropriate Organization, Product, and FAQ structured data only where the necessary information is present.
- Do not invent ratings, reviews, awards, company facts, prices, availability, or other unsupported information.
- Ensure structured data values stay consistent with PRODUCTS.

4. PERFORMANCE
- Avoid unnecessary JavaScript work.
- Avoid duplicate event listeners.
- Avoid unnecessary DOM re-renders where practical.
- Keep the single-file architecture.
- Do not add frameworks or external libraries.
- Keep image paths and existing functionality intact for now.

5. HTML QUALITY
- Check for duplicate IDs.
- Check for missing required attributes.
- Check links and buttons for appropriate behavior.
- Check that forms have usable labels and controls.
- Keep the approved legal footer paragraph exactly unchanged.

STRICT CONSTRAINTS:
- Do not modify the API object between API START and API END.
- Do not modify PRODUCTS IDs, names, prices, stock, categories, or descriptions.
- Do not break cart, coupon, search, filter, sort, quick view, delivery, newsletter, or checkout behavior.
- Keep CSS and JavaScript inside index.html.
- Do not add frameworks or libraries.
- Do not replace product images yet.
- Do not invent unsupported business information.

After implementation:
1. Explain every accessibility improvement.
2. Explain every SEO improvement.
3. Explain any responsive fixes.
4. Run syntax/basic verification.
5. Tell me exactly what must be manually checked in the browser.
6. Do not claim mobile accessibility or screen-reader behavior was verified unless it was actually tested.

## Prompt 7

- Tool: Codex
- Type: code
- Prompt:

Before making any code changes, inspect the existing PRODUCTS array in index.html.

Do not modify anything.

Give me only this information in a numbered list for all 8 products:
1. Product ID
2. Exact product name
3. Exact category
4. Exact description
5. Current image path

Do not change index.html, API, PRODUCTS, CSS, JavaScript, or any files.


## Prompt 8

- Tool: Codex
- Type: code
- Prompt:


## Hero Image Integration Only

Inspect the existing index.html and integrate the new hero image:

images/hero.png

IMPORTANT:
- Only work on the hero section.
- Do NOT modify the PRODUCTS array.
- Do NOT modify any product image paths.
- Do NOT modify p101.png through p108.png.
- Do NOT modify API logic.
- Do NOT modify cart, coupon, search, filter, sort, checkout, delivery, newsletter, or countdown logic.
- Do NOT modify the approved legal paragraph.
- Do NOT redesign the rest of the website.
- Do NOT generate or rename any images.

Hero requirements:
1. Use `images/hero.png` as the main hero visual.
2. Keep the existing hero section and its CTA/navigation behavior working.
3. Make the image responsive on desktop, tablet, and mobile.
4. Make sure the image does not cause horizontal scrolling.
5. Preserve a good aspect ratio; do not unnecessarily stretch or distort the image.
6. Because the hero image already contains its own visual text/content, do not duplicate the same text over it.
7. Add appropriate alt text for the hero image.
8. Keep the existing premium MistVale visual style.

After the change:
- Run a JavaScript syntax check.
- Verify `images/hero.png` exists and is referenced correctly.
- Verify the 8 product image paths remain unchanged.
- Verify PRODUCTS data remains unchanged.
- Verify no business logic was modified.

Report exactly:
- files changed
- hero changes made
- verification performed
- any remaining manual browser check

Do not modify NOTES.md or PROMPTS.md in this step.



So Completed our Project