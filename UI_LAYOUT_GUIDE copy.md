```
╔═══════════════════════════════════════════════════════════════╗
║                                                               ║
║   📄 Document Summarizer                                      ║
║   ─────────────────────────────────────────────────────────   ║
║                                                               ║
║   Quickly summarize your documents with AI-powered           ║
║   insights. Upload any text document and get a concise,      ║
║   easy-to-read summary in seconds.                           ║
║                                                               ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║   📋 How to use:                                              ║
║   1. Click 'Upload Document' to select your file             ║
║   2. Wait for the summarization to complete                  ║
║   3. Review the summary and use action buttons               ║
║                                                               ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║   [📁 Upload Document]                                        ║
║                                                               ║
║   📎 File: example.pdf (245.32 KB)                            ║
║                                                               ║
║   ⏳ Processing your document...                              ║
║                                                               ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║   ✨ Summarized Content                                       ║
║   ───────────────────────────────────────────────────────    ║
║                                                               ║
║   Lorem ipsum dolor sit amet, consectetur adipiscing         ║
║   elit. Sed do eiusmod tempor incididunt ut labore et        ║
║   dolore magna aliqua. Ut enim ad minim veniam, quis         ║
║   nostrud exercitation ullamco laboris nisi ut aliquip       ║
║   ex ea commodo consequat...                                 ║
║                                                               ║
║   [📋 Copy Summary]  [📥 Download]                           ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

# UI Layout Map

## Responsive Structure

### Desktop View (Full Width)
```
┌─ Header (Text1) ────────────────────────────────┐
├─ Subtitle (SubtitleText) ───────────────────────┤
├─ Instructions (InstructionText) ────────────────┤
├─ Upload Area (FilePicker1) ─────────────────────┤
├─ File Info (FileInfoText) ──────────────────────┤
├─ Status (StatusText) ───────────────────────────┤
├─ Summary (RichTextEditor1) ─────────────────────┤
└─ Actions (CopyButton | DownloadButton) ─────────┘
```

### Mobile View (Stacked)
```
┌─ Header ──────────────┐
├─ Subtitle ────────────┤
├─ Instructions ────────┤
├─ Upload ──────────────┤
├─ File Info ───────────┤
├─ Status ──────────────┤
├─ Summary ─────────────┤
└─ Actions (Stacked) ───┘
```

## Widget Positioning Details

| Widget | Row Start | Row End | Visibility | Purpose |
|--------|-----------|---------|-----------|---------|
| Text1 (Title) | 1 | 7 | Always | Main heading |
| SubtitleText | 7 | 9 | Always | Description |
| InstructionText | 10 | 18 | Always | User guidance |
| FilePicker1 | 22 | 27 | Always | File upload |
| FileInfoText | 30 | 32 | When file selected | File confirmation |
| StatusText | 34 | 36 | When processing/done | Progress indicator |
| RichTextEditor1 | 50 | 80 | When summary ready | Result display |
| CopyButton | 46 | 48 | When summary ready | Copy action |
| DownloadButton | 46 | 48 | When summary ready | Download action |

## Data Flow

```
FilePicker1 (User selects file)
    ↓
triggers onFilesSelected: SummarizeDoc.run()
    ↓
SummarizeDoc Query Executes
    ↓
StatusText shows loading indicator
(SummarizeDoc.isLoading = true)
    ↓
API processes document
    ↓
SummarizeDoc completes
(SummarizeDoc.data populated)
    ↓
RichTextEditor1 displays summary
(defaultText binds to SummarizeDoc.data)
    ↓
CopyButton and DownloadButton enabled
(isDisabled: !SummarizeDoc.data)
    ↓
User clicks Copy/Download
    ↓
Action executed with user feedback
```

## Conditional Rendering Logic

### FileInfoText Visibility
```javascript
isVisible: {{FilePicker1.files.length > 0}}
```
Shows only after user selects a file

### StatusText Visibility & Content
```javascript
isVisible: {{SummarizeDoc.isLoading || SummarizeDoc.data}}
text: {{SummarizeDoc.isLoading ? 
  '⏳ Processing your document...' : 
  SummarizeDoc.data ? 
  '✅ Summary ready!' : 
  ''
}}
```

### Summary Display Visibility
```javascript
isVisible: {{SummarizeDoc.data}}
isLoading: {{SummarizeDoc.isLoading}}
```

### Button States
```javascript
isDisabled: {{!SummarizeDoc.data}}
isVisible: {{SummarizeDoc.data}}
```
Buttons are hidden and disabled until summary is available

## Color & Styling Guide

### Text Elements
- **Title**: Bold, 1rem, #231F20 (dark), responsive layout
- **Subtitle**: 0.875rem, #626262 (medium gray)
- **Instructions**: Boxed, light shadow, HTML formatted
- **File Info**: 0.875rem, #626262 (medium gray)
- **Status**: Dynamic color (amber while loading, green when done)

### Buttons
- **Copy Button**: Primary color (theme), light shadow
- **Download Button**: Green (#03B365), light shadow
- **Both**: 40px height, 120px minimum width, round corners

### Layout
- **Main width**: Full width responsive
- **Left padding**: 2 columns (consistent margin)
- **Border radius**: Theme-based
- **Shadows**: Light box shadows for depth

## Accessibility Features

✓ Clear icon usage with text labels
✓ Descriptive button labels
✓ Color not sole indicator (uses icons too)
✓ Proper contrast ratios
✓ Semantic HTML in rich text editor
✓ Mobile-friendly touch targets (40px+ buttons)
✓ Loading states clearly indicated
✓ Disabled state obvious on buttons
