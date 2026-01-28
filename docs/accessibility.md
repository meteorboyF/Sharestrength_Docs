# Accessibility

ShareStrength is built with accessibility as a core feature, designed to serve People With Disabilities (PWD).

## Accessibility Settings

Each user can customize their accessibility preferences:

| Setting | Description | Options |
|---------|-------------|---------|
| Font Size | Text size adjustment | Small, Medium, Large, Extra Large |
| Text-to-Speech (TTS) | Read content aloud | Enabled / Disabled |
| Speech-to-Text (STT) | Voice input | Enabled / Disabled |
| High Contrast Mode | Enhanced visual contrast | Enabled / Disabled |

## Managing Settings

### Via API

```http
PUT /api/v1/profile
Authorization: Bearer {token}
Content-Type: application/json

{
    "accessibility_settings": {
        "font_size": "large",
        "tts_enabled": true,
        "stt_enabled": true,
        "high_contrast": false
    }
}
```

### Via Livewire

Settings are managed through the `MyProfile` Livewire component with real-time updates.

## Accessible Resources

The Resource Library provides materials in multiple accessible formats:

### Resource Types

- **Audiobooks** - Audio versions of text content
- **Sign Language Videos** - Video content with sign language
- **Braille Materials** - Documents formatted for braille displays
- **Large Print** - Documents with enlarged text
- **Accessible PDFs** - PDFs with proper tagging and structure

### Resource Metadata

Each resource includes:

- File size information
- Duration (for audio/video)
- Format type
- Accessibility features

## UI Accessibility Features

### Keyboard Navigation

All interactive elements are keyboard accessible:

- `Tab` - Navigate between elements
- `Enter` / `Space` - Activate buttons and links
- `Escape` - Close modals and dialogs

### Screen Reader Support

- Proper ARIA labels on interactive elements
- Semantic HTML structure
- Alt text for images
- Clear heading hierarchy

### Visual Design

- Sufficient color contrast ratios
- Clear focus indicators
- Consistent layout patterns
- Responsive design for various screen sizes

## Tailwind CSS Accessibility

The frontend uses Tailwind CSS with accessibility utilities:

```html
<!-- Focus visible for keyboard users -->
<button class="focus:outline-none focus-visible:ring-2 focus-visible:ring-blue-500">
    Submit
</button>

<!-- Screen reader only text -->
<span class="sr-only">Close menu</span>

<!-- High contrast mode styles -->
<div class="high-contrast:bg-black high-contrast:text-white">
    Content
</div>
```

## Best Practices

### For Developers

1. **Always include alt text** for images
2. **Use semantic HTML** elements (`button`, `nav`, `main`, etc.)
3. **Provide visible focus states** for keyboard users
4. **Test with screen readers** (NVDA, VoiceOver, JAWS)
5. **Ensure sufficient color contrast** (WCAG 2.1 AA minimum)

### For Content Creators

1. **Use clear, simple language**
2. **Provide text alternatives** for non-text content
3. **Structure content with headings**
4. **Use descriptive link text** (not "click here")

## Database Schema

```sql
CREATE TABLE accessibility_settings (
    id BIGINT PRIMARY KEY,
    user_id BIGINT NOT NULL,
    font_size VARCHAR(255) DEFAULT 'medium',
    tts_enabled BOOLEAN DEFAULT FALSE,
    stt_enabled BOOLEAN DEFAULT FALSE,
    high_contrast BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP,
    updated_at TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

## Testing Accessibility

### Automated Testing

```bash
# Run accessibility audit with Lighthouse
npx lighthouse http://localhost:8000 --only-categories=accessibility

# Run axe-core tests
npm run test:a11y
```

### Manual Testing Checklist

- [ ] Navigate entire app using only keyboard
- [ ] Test with screen reader enabled
- [ ] Verify all images have alt text
- [ ] Check color contrast ratios
- [ ] Test with zoom at 200%
- [ ] Verify form labels are associated with inputs
