# Accessibility Checklist
## RREMOGO

### 1. Purpose

This checklist evaluates whether the Student Rental Marketplace can be accessed and used by people with different abilities, including users who rely on keyboards, screen readers, magnification, or other assistive technologies.

The checklist uses the Web Content Accessibility Guidelines (WCAG) 2.2 Level AA as a target for review. Completing this checklist alone does not establish full WCAG conformance.

### 2. Accessibility Checklist

Status definitions:
- **Pass:** Checked and meets the criterion.
- **Fail:** Checked and requires improvement.
- **Not tested:** No verification has been completed.
- **Not applicable:** The criterion does not apply to the feature.

| ID | Check | How to verify | Status |
|---|---|---|---|
| A1 | Images have meaningful alternative text. | Inspect item images and screen-reader output. | Not tested |
| A2 | All functions are keyboard accessible. | Navigate using Tab, Shift+Tab, Enter, and Space. | Not tested |
| A3 | Keyboard focus is visible. | Check the focus indicator on links, buttons, and inputs. | Not tested |
| A4 | Text has sufficient colour contrast. | Test foreground and background colours using a contrast checker. | Not tested |
| A5 | Information is not conveyed by colour alone. | Check status indicators, errors, and availability labels. | Not tested |
| A6 | Form fields have clear labels. | Inspect registration, login, listing, and rental forms. | Not tested |
| A7 | Required fields are identified clearly. | Check item listings and rental-request forms. | Not tested |
| A8 | Error messages explain how to correct mistakes. | Submit invalid data and inspect the feedback. | Not tested |
| A9 | The interface supports browser zoom and text resizing. | Zoom to 200% and check for lost content or functionality. | Not tested |
| A10 | Pages reflow on narrow screens. | Test at a width equivalent to 320 CSS pixels. | Not tested |
| A11 | Headings follow a logical hierarchy. | Inspect page headings and document structure. | Not tested |
| A12 | Links and buttons have descriptive names. | Check whether their purpose is understandable out of context. | Not tested |
| A13 | Status changes are communicated to assistive technology. | Test rental-status updates and notification messages with a screen reader. | Not tested |
| A14 | Authentication is accessible. | Check login, validation, and email-verification steps. | Not tested |
| A15 | Time limits can be understood and addressed. | Review session expiry and the 48-hour pending-payment rule; communicate deadlines clearly. | Not tested |
| A16 | Chat can be used without a mouse. | Navigate the chat controls and message input using the keyboard. | Not tested |
| A17 | Date and return information is clear. | Check that due dates use understandable date formats and are labelled. | Not tested |
| A18 | Touch targets are adequately sized. | Inspect interactive controls against WCAG 2.2 target-size requirements and exceptions. | Not tested |

### 3. Accessibility Testing Procedure

1. Navigate the main pages using only the keyboard.
2. Test login, registration, item search, rental requests, payment-proof upload, and return confirmation.
3. Inspect text and control contrast with a contrast-checking tool.
4. Test page layout at 200% zoom and on a narrow viewport.
5. Use a screen reader to inspect form labels, buttons, images, and status messages.
6. Record each result and create an issue for every failed check.
7. Retest failed checks after implementing improvements.

### 4. Acceptance Criteria

The prototype should aim to:
- Provide keyboard access to all essential rental tasks.
- Display visible keyboard focus.
- Label form controls and communicate validation errors clearly.
- Provide meaningful alternative text for informative images.
- Meet applicable WCAG 2.2 Level AA contrast requirements.
- Remain usable when zoomed and on small screens.
- Communicate rental status, payment confirmation, and return deadlines clearly.

### 5. Results Summary

| Result | Number |
|---|---:|
| Checks passed | To be recorded |
| Checks failed | To be recorded |
| Checks not tested | To be recorded |
| Checks not applicable | To be recorded |

### 6. Conclusion

Accessibility should be reviewed throughout development rather than only at the end. The checklist will help identify barriers and guide improvements to the Student Rental Marketplace. The final report should reflect actual testing results and any unresolved accessibility issues.

World Wide Web Consortium (W3C). *Web Content Accessibility Guidelines (WCAG) 2.2.*

https://www.w3.org/TR/WCAG22/
