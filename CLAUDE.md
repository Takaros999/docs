# World ID Documentation - Mintlify Migration Guide

This document contains important learnings and guidelines from migrating World ID documentation to Mintlify.

## Project Structure

- **Working Directory**: `/Users/panagiotis.kakalis/Repos/docs`
- **Old Documentation Source**: `/Users/panagiotis.kakalis/Repos/world-id-docs`
- **Main Configuration**: `docs.json` (Mintlify configuration)

## Key Migration Learnings

### 1. Component Conversions

When migrating from the old Next.js-based docs to Mintlify, use these component mappings:

- `<Note>` → Use Mintlify's built-in `<Note>`, `<Info>`, `<Warning>`, `<Tip>` components
- `<CodeGroup>` → Wrap code blocks with `<CodeGroup>` tags (Mintlify syntax)
- Custom card components → Use HTML with Tailwind CSS classes
- Tables with images → Use flex layouts within table cells for better alignment

### 2. Image Handling

**Important**: Mintlify expects images at the root level, not in `/public/`:
- Store images in `/images/`, `/icons/`, `/logo/` directories (root level)
- Reference images without the `/public/` prefix: `src="/images/example.png"`
- When migrating, copy from `/public/images/` to `/images/`

### 3. Table Formatting

For tables with icons and text that need to be aligned:
```markdown
| <div className="flex items-center gap-2"><img src="/icons/icon.svg" alt="Alt" className="w-6 h-6" /><span className="font-semibold">Text</span></div> | Value |
```

To prevent header text wrapping:
```markdown
| <span className="whitespace-nowrap">Long Header Text</span> | Description |
```

### 4. Navigation Structure

The navigation is configured in `docs.json`:
- Use tabs for major sections (Overview, World ID, etc.)
- Group related pages within each tab
- Maintain the same URL structure as the old docs for consistency

### 5. Common Issues & Solutions

1. **Images not displaying**: Move images from `/public/` to root-level directories
2. **500 errors on pages**: Usually caused by complex HTML/React components - simplify to markdown or basic HTML
3. **CodeGroup formatting**: Ensure all code blocks within a group are properly wrapped
4. **Duplicate titles**: Remove H1 headings if using frontmatter titles

## Testing Commands

When making changes, test with:
```bash
# Run linting if available
npm run lint

# Run type checking if available  
npm run typecheck
```

## File Structure

```
/docs/
├── docs.json           # Main Mintlify configuration
├── index.mdx          # Homepage
├── favicon.svg
├── /world-id/         # World ID documentation
│   ├── index.mdx
│   ├── concepts.mdx
│   ├── /id/           # ID Kit documentation
│   ├── /sign-in/      # Sign in with World ID
│   └── /reference/    # Technical reference
├── /images/           # Image assets
├── /icons/            # Icon assets
└── /logo/             # Logo files
```

## Important Notes

1. Always perform 1:1 content migration first, then optimize
2. Test all image paths after migration
3. Verify navigation works correctly in docs.json
4. Check that all CodeGroup components render properly
5. Ensure tables display correctly on mobile devices

## Future Improvements

- Consider implementing a script to automate component conversions
- Add CI/CD checks for broken image links
- Create templates for common page layouts

## API Documentation Best Practices

When documenting APIs in Mintlify, use these native components for better readability:

### 1. ParamField Component
Replace parameter tables with `<ParamField>` components:
```jsx
<ParamField body="nullifier_hash" type="string" required>
  The unique user identifier (nullifier hash), as provided by IDKit.
</ParamField>
```

### 2. CodeGroup Component
Group related code examples:
```jsx
<CodeGroup>
```bash cURL
curl -X POST "/api/v2/verify/{app_id}" \
    -H "Content-Type: application/json"
```

```javascript JavaScript
fetch(apiUrl, {
  method: 'POST',
  headers: {'Content-Type': 'application/json'}
})
```
</CodeGroup>
```

### 3. Tabs Component
Organize multiple responses or options:
```jsx
<Tabs>
  <Tab title="200 OK">
    ```json
    {"success": true}
    ```
  </Tab>
  <Tab title="400 Bad Request">
    ```json
    {"error": "Invalid request"}
    ```
  </Tab>
</Tabs>
```

### 4. Endpoint Display
Always show the endpoint clearly at the top of each API section:
```
<CodeGroup>
```bash Endpoint
POST /api/v2/verify/{app_id}
```
</CodeGroup>
```

## Mintlify Documentation Reference

Always reference the Mintlify documentation when migrating or creating new docs:
https://mintlify.com/docs/llms-full.txt

Key sections to reference:
- Components: For available UI components like ParamField, CodeGroup, Tabs
- API References: For best practices in documenting APIs
- Content Organization: For structuring documentation effectively

## Navigation and Page Titles

### Important: Always Add Custom Titles
Mintlify auto-generates page titles from filenames if no title is specified in frontmatter, which can cause case sensitivity issues. Always add custom titles:

```markdown
---
title: "Your Custom Title"
---
```

### Title Guidelines
- Use proper capitalization (e.g., "IDKit" not "Idkit")
- Be consistent with product names (e.g., "World ID" not "world-id")
- Use descriptive titles that match the content
- For technical references, include the type (e.g., "API Reference", "Smart Contracts")

## IDKit Documentation Best Practices

When documenting SDK/library references like IDKit:

### 1. Use Properties Component for Parameters
Replace parameter tables with Mintlify's `<Properties>` component:
```jsx
<Properties>
  <Property name="app_id" type="string" required>
    Unique identifier for the app verifying the action.
  </Property>
</Properties>
```

### 2. Group Related Sections
- Components and their props
- Hooks and their usage
- Types and interfaces
- Error handling

### 3. Code Examples with Focus
Use the `focus` attribute to highlight important lines:
```jsx
```jsx {3-7}
// Important lines will be highlighted
```