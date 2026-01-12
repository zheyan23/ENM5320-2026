# Instructions for Converting Markdown/LaTeX to HTML with KaTeX

This document provides guidelines for converting course material written in Markdown with LaTeX math notation into properly formatted HTML pages with KaTeX rendering, maintaining a consistent style across the ENM5320 course website.

## Template Structure

All course content pages should follow this structure:

### 1. HTML Boilerplate with KaTeX Support

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>[Page Title] - ENM5320</title>
    
    <!-- KaTeX CSS and JS -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css">
    <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js"></script>
    <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js"></script>
    
    <style>
        /* See CSS section below */
    </style>
</head>
<body>
    <a href="../../index.html" class="back-link">← Back to Course Schedule</a>
    
    <!-- Content here -->
    
    <script>
        document.addEventListener("DOMContentLoaded", function() {
            renderMathInElement(document.body, {
                delimiters: [
                    {left: "$$", right: "$$", display: true},
                    {left: "\\[", right: "\\]", display: true},
                    {left: "$", right: "$", display: false},
                    {left: "\\(", right: "\\)", display: false}
                ],
                throwOnError: false
            });
        });
    </script>
</body>
</html>
```

### 2. Standard CSS Styling

Use this CSS to maintain Penn branding and consistent typography:

```css
body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
    line-height: 1.6;
    color: #333;
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
    background: #fff;
}

h1 {
    color: #011F5B;  /* Penn Blue */
    border-bottom: 3px solid #990000;  /* Penn Red */
    padding-bottom: 10px;
    margin-top: 30px;
}

h2 {
    color: #011F5B;
    margin-top: 30px;
    border-bottom: 2px solid #e0e0e0;
    padding-bottom: 5px;
}

h3 {
    color: #011F5B;
    margin-top: 20px;
}

p {
    text-align: justify;
    margin-bottom: 15px;
}

ul, ol {
    margin-bottom: 15px;
}

li {
    margin-bottom: 8px;
}

.katex-display {
    margin: 1.5em 0;
    overflow-x: auto;
    overflow-y: hidden;
}

.definition {
    background: #f8f9fa;
    border-left: 4px solid #82AFD3;
    padding: 15px;
    margin: 20px 0;
    border-radius: 4px;
}

.summary-box {
    background: #fff3cd;
    border: 1px solid #F2C100;
    padding: 15px;
    margin: 20px 0;
    border-radius: 4px;
}

.example-box {
    background: #e8f4f8;
    border-left: 4px solid #82AFD3;
    padding: 15px;
    margin: 20px 0;
    border-radius: 4px;
}

.theorem-box {
    background: #f0f0f0;
    border: 2px solid #011F5B;
    padding: 15px;
    margin: 20px 0;
    border-radius: 4px;
}

strong {
    color: #011F5B;
}

.back-link {
    display: inline-block;
    margin-bottom: 20px;
    color: #82AFD3;
    text-decoration: none;
}

.back-link:hover {
    text-decoration: underline;
}

code {
    background: #f5f5f5;
    padding: 2px 6px;
    border-radius: 3px;
    font-family: 'Courier New', monospace;
    font-size: 0.9em;
}

pre {
    background: #f5f5f5;
    padding: 15px;
    border-radius: 4px;
    overflow-x: auto;
}
```

## Converting Math Notation

### Inline Math
- Markdown: `$x^2$` or `\(x^2\)`
- HTML: `\(x^2\)` (KaTeX will render automatically)

### Display Math
- Markdown: `$$x^2$$` or `\[x^2\]`
- HTML: `$$x^2$$` (KaTeX will render automatically)

**Important:** In HTML, use `\(` and `\)` for inline math, `$$` for display math. Do NOT escape the backslashes in the actual HTML file.

## Common LaTeX Patterns

### Vectors and Matrices
```latex
\mathbf{x}              % bold vector
\mathbf{A}              % bold matrix
\mathbb{R}^n            % real n-dimensional space
\|\cdot\|               % norm notation
\langle x, y \rangle    % inner product
```

### Greek Letters
```latex
\alpha, \beta, \gamma   % lowercase
\Delta, \Gamma          % uppercase
\epsilon, \varepsilon   % epsilon variants
```

### Common Operators
```latex
\leq, \geq              % less/greater than or equal
\max, \min, \sup, \inf  % optimization
\sum_{i=1}^n            % summation
\int_0^t                % integral
\frac{a}{b}             % fraction
```

## Document Structure Guidelines

### Main Title (H1)
- Use once at the top of the document
- Should be descriptive: "Topic Name" or "Topic: Subtopic"

### Major Sections (H2)
- Use for main conceptual divisions
- Examples: "Definitions", "Theorems", "Examples", "Exercises"

### Subsections (H3)
- Use for specific topics within sections
- Examples: specific theorem names, example numbers

### Special Boxes

**For Definitions:**
```html
<div class="definition">
    <strong>Definition (Name):</strong> Content here with inline math \(x\) and display math:
    $$E = mc^2$$
</div>
```

**For Theorems:**
```html
<div class="theorem-box">
    <strong>Theorem (Name):</strong> Statement here.
    <p><em>Proof:</em> Proof content here.</p>
</div>
```

**For Examples:**
```html
<div class="example-box">
    <strong>Example:</strong> Example content here.
</div>
```

**For Important Summaries:**
```html
<div class="summary-box">
    <strong>Key Points:</strong>
    <ul>
        <li>Point 1</li>
        <li>Point 2</li>
    </ul>
</div>
```

## File Naming and Organization

### Directory Structure
```
NewMaterial/
  LectureXX_MonDD/
    TopicName.html
    figures/
      figure1.png
      figure2.svg
```

### File Naming
- Use descriptive names: `Analysis.html`, `FiniteDifferences.html`, `StabilityTheory.html`
- Use PascalCase for multi-word names
- Avoid spaces and special characters

## Navigation

### Back Link
Always include at the top:
```html
<a href="../../index.html" class="back-link">← Back to Course Schedule</a>
```

Adjust the relative path based on your location in the directory structure.

## Color Scheme

Maintain Penn branding throughout:
- **Penn Blue:** `#011F5B` (headings, strong text, borders)
- **Penn Red:** `#990000` (accents, H1 underline)
- **Accent Blue:** `#82AFD3` (links, definition boxes)
- **Warning Yellow:** `#F2C100` (summary boxes)
- **Light Gray:** `#f8f9fa` (backgrounds)
- **Border Gray:** `#e0e0e0` (subtle borders)

## Example Conversion

### From Markdown:
```markdown
# Vector Spaces

## Definition

A **vector space** over $\mathbb{R}$ is a set $V$ with operations...

$$\|\mathbf{x} + \mathbf{y}\| \leq \|\mathbf{x}\| + \|\mathbf{y}\|$$
```

### To HTML:
```html
<h1>Vector Spaces</h1>

<h2>Definition</h2>

<div class="definition">
    <strong>Definition (Vector Space):</strong> A <strong>vector space</strong> over \(\mathbb{R}\) is a set \(V\) with operations...
</div>

<p>The triangle inequality states:</p>
$$\|\mathbf{x} + \mathbf{y}\| \leq \|\mathbf{x}\| + \|\mathbf{y}\|$$
```

## Testing

Before publishing, verify:
1. ✓ All math renders correctly (no red error boxes)
2. ✓ Page is responsive (test on mobile width)
3. ✓ Back link works and points to correct location
4. ✓ All colors match Penn branding
5. ✓ Content is readable and properly formatted
6. ✓ External links open in new tabs (if applicable)

## Common Mistakes to Avoid

1. **Don't** escape backslashes in HTML: use `\(x\)` not `\\(x\\)`
2. **Don't** mix inline and display delimiters inconsistently
3. **Don't** forget the KaTeX auto-render script at the bottom
4. **Don't** use absolute paths - use relative paths for internal links
5. **Don't** forget to set proper page titles in `<title>` tag
6. **Don't** mix styling - use provided CSS classes consistently

## Quick Checklist

When creating a new page:
- [ ] Copy template with KaTeX support
- [ ] Update page title
- [ ] Add back link with correct relative path
- [ ] Convert all math notation properly
- [ ] Use appropriate box styles for special content
- [ ] Maintain Penn color scheme
- [ ] Test math rendering
- [ ] Verify all links work
- [ ] Check mobile responsiveness

## Reference Files

See `NewMaterial/Lecture01_Jan15/Analysis.html` for a complete working example following these guidelines.
