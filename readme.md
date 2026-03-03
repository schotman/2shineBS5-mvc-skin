

# This is a custom MVC version of 2shine skin from 2sic.

See:
https://github.com/2sic/dnn-theme-2shine-bs5
https://2shine.org/

This document explains the MVC skin version in this theme, how it maps to the ASCX version, and what to keep in sync when maintaining both.

## Overview

This skin includes two implementation styles:

1. ASCX skin files (`*.ascx`) and shared controls in `controls/`
2. MVC skin files (`Views/*.cshtml`) with partials and MVC helpers

Both versions are intended to render the same layout variants and user experience wherever possible.

## ASCX to MVC Mapping

### Layout files

1. `default.ascx` -> `Views/default.cshtml`
2. `Centered.ascx` -> `Views/Centered.cshtml`
3. `Centered-Submenu.ascx` -> `Views/Centered-Submenu.cshtml`
4. `Fullscreen.ascx` -> `Views/Fullscreen.cshtml`
5. `Float-HeaderWide.ascx` -> `Views/Float-HeaderWide.cshtml`
6. `popUpSkin.ascx` -> `Views/PopUpSkin.cshtml`

### Shared structure

1. `controls/theme-body.ascx` -> split into:
   1. `Views/_Header.cshtml`
   2. `Views/_MainSection.cshtml`
   3. `Views/_Footer.cshtml`
2. `controls/Breadcrumb.ascx` -> `Views/_BreadcrumbNavigation.cshtml`
3. `controls/body-css-classes.ascx` -> `Views/_BodyCssClassSetup.cshtml`
4. `controls/LanguageNavigation.ascx` -> `@Html.Language()` in `Views/_Header.cshtml`
5. `controls/register.ascx` -> no 1:1 MVC file (registration concerns are handled by MVC helpers/partials)

## Parity Expectations

These areas should generally stay aligned between ASCX and MVC:

1. Layout structure and CSS class conventions
2. Pane placement and naming intent
3. Navigation behavior and breadcrumb visibility rules
4. Footer content and utility links
5. Resource include order (`fonts`, `theme.css`, `dnn-default`, main styles and scripts)

## Known Differences

Current known differences to be aware of:

1. Sidebar rendering in `Centered-Submenu`:
   1. ASCX only renders sidebar when child pages are available.
   2. MVC currently sets `renderSidebar = showSidebarNavigation` (the `hasVisibleChildren` condition is not active).
2. Header pane empty-state class:
   1. ASCX can add `theme-header-pane-empty`.
   2. MVC `_Header.cshtml` does not currently add this class.
3. Language switcher implementation:
   1. ASCX uses `controls/LanguageNavigation.ascx` custom filtering logic.
   2. MVC uses `@Html.Language()`.
4. SEO page title optimization:
   1. ASCX includes `controls/optimize-page-title.ascx` via `controls/register.ascx`.
   2. MVC does not include an equivalent helper/partial today.
5. 2sxc QuickEdit:
   1. ASCX includes `controls/2sxc-quickedit.ascx` via `controls/register.ascx`.
   2. MVC does not include an equivalent helper/partial today.
6. Popup skin rendering details:
   1. ASCX popup skin is only a pane container.
   2. MVC popup skin adds an inline body background style and a pane css class.

## Footer Note

Footer content has been aligned so ASCX mirrors MVC footer intent:

1. Address area now uses the copyright skin object.
2. Imprint area uses user/login/terms/privacy skin objects.
3. Hardcoded ASCX footer microformat contact block has been removed in favor of shared behavior.

## How to Keep Both Versions In Sync

When changing shared UX, update both implementations in the same change set:

1. If layout wiring changes:
   1. update the corresponding root `*.ascx` file
   2. update the matching `Views/*.cshtml` layout
2. If header/main/footer markup changes:
   1. update `controls/theme-body.ascx`
   2. update the corresponding MVC partial(s)
3. If behavior logic changes (sidebar conditions, breadcrumb rules, language behavior):
   1. update ASCX code-behind/script block logic
   2. update equivalent MVC view logic
4. If DNN skin object output changes in ASCX:
   1. verify CSS impacts for `.Normal` and related classes
   2. keep MVC helper output styling consistent



