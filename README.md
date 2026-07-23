# mobile-bottom-nav

Reusable mobile bottom navigation bar for React apps. Router-agnostic, Tailwind-based, PWA-ready.

Synthesized from patterns across 5 production apps: scrap-sync, project-dashboard, Playbook-Task-App, Haven, and idea-foundry.

## Features

- **Router-agnostic** — pass `navigate` and `currentPath` from any router (wouter, react-router, Next.js, Tanstack)
- **Hidden on desktop** — `md:hidden` CSS gate, no JS breakpoint check, no hydration flash
- **Safe-area aware** — `env(safe-area-inset-bottom)` padding for iPhone home indicator
- **Height reporting** — publishes `--mobile-nav-h` CSS var via `ResizeObserver` so full-screen surfaces can offset
- **Optional center FAB** — elevated circular button between items
- **Badges** — numeric pills and dot indicators with screen-reader context
- **Accessibility** — `aria-current="page"`, `aria-label` with badge context, 44px+ touch targets, focus-visible rings
- **Dark mode** — works with Tailwind `dark:` classes
- **Backdrop blur** — readable over scrolling content

## Install

Copy `src/` into your project, or add as a local dependency:

```json
"dependencies": {
  "mobile-bottom-nav": "file:../mobile-bottom-nav"
}
```

### Required CSS

Add to your global CSS:

```css
.safe-area-pb { padding-bottom: env(safe-area-inset-bottom); }
```

## Usage

```tsx
import { BottomNav } from "mobile-bottom-nav";
import { Home, Plus, Settings, LogOut } from "lucide-react";

function Layout() {
  // wouter example
  const [location, setLocation] = useLocation();

  return (
    <BottomNav
      currentPath={location}
      navigate={(href) => (e) => {
        e.preventDefault();
        setLocation(href);
      }}
      items={[
        { label: "Home", icon: <Home className="w-5 h-5" />, href: "/app", matchPaths: ["/app"] },
        { label: "New", icon: <Plus className="w-5 h-5" />, href: "/app/new" },
        { label: "Settings", icon: <Settings className="w-5 h-5" />, href: "/app/settings" },
      ]}
      actions={[
        { label: "Sign Out", icon: <LogOut className="w-5 h-5" />, onClick: handleSignOut },
      ]}
    />
  );
}
```

### With center FAB

```tsx
<BottomNav
  currentPath={location}
  navigate={navigateFn}
  items={[
    { label: "Home", icon: <HomeIcon />, href: "/dashboard", matchPaths: ["/dashboard"] },
    { label: "Cases", icon: <CasesIcon />, href: "/cases" },
    { label: "Parts", icon: <PartsIcon />, href: "/parts" },
  ]}
  fab={{
    label: "Ask AI",
    icon: <SparklesIcon />,
    onClick: () => setChatOpen(true),
  }}
  actions={[
    { label: "More", icon: <MoreIcon />, onClick: () => setMoreOpen(true), ariaExpanded: moreOpen },
  ]}
/>
```

### Using `--mobile-nav-h` for full-screen surfaces

```css
.my-modal {
  position: fixed;
  bottom: var(--mobile-nav-h, 0px);
  left: 0;
  right: 0;
}
```

## API

### `<BottomNav>`

| Prop | Type | Description |
|------|------|-------------|
| `currentPath` | `string` | Current route path for active-state matching |
| `navigate` | `(href) => (e) => void` | Navigate function from your router |
| `items` | `BottomNavItemConfig[]` | Link items (Home, New, Settings) |
| `actions` | `BottomNavItemConfig[]` | Action buttons appended after items (Sign Out, More) |
| `fab` | `{ label, icon, onClick, ariaLabel? }` | Optional center FAB |
| `activeClass` | `string` | Active item color class (default: `text-primary`) |
| `inactiveClass` | `string` | Inactive item color class (default: `text-muted-foreground`) |

### `BottomNavItemConfig`

| Prop | Type | Description |
|------|------|-------------|
| `href` | `string?` | Destination path (omit for action buttons) |
| `onClick` | `() => void` | Click handler (for action buttons) |
| `label` | `string` | Visible label text |
| `icon` | `ReactNode` | Icon element (~24px) |
| `matchPaths` | `string[]?` | Exact paths that mark item active |
| `badgeCount` | `number?` | Numeric badge (shows pill when > 0) |
| `badgeDot` | `boolean?` | Dot badge indicator |
| `badgeNoun` | `string?` | SR noun for badge: "Cases, 3 urgent" |
| `ariaLabel` | `string?` | Override accessible name |
| `ariaExpanded` | `boolean?` | For popup triggers |
| `ariaControls` | `string?` | Pairs with ariaExpanded |

## License

MIT
