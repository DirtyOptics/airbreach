# 🖼️ Image Cheatsheet

A comprehensive reference for working with images in MkDocs Material.

---

## 📁 Image Storage

**Location:** `docs/images/`  
**Usage from root:** `![alt text](images/filename.jpg)`  
**Usage from subfolder:** `![alt text](../images/filename.jpg)`

---

## 🎯 Basic Image Syntax

### Standard Markdown
```markdown
![Alt text](../images/image1.png)
![Alt text](../images/image2.jpg "Optional title")
```

**Live Examples:**
![Sample Image 1](../images/image1.png)
![Sample Image 2](../images/image2.jpg "This is a title")

### With Width Control
```markdown
![Alt text](../images/image1.png){ width="300" }
![Alt text](../images/image2.jpg){ width="50%" }
```

**Live Examples:**
![Sample Image 1](../images/image1.png){ width="300" }
![Sample Image 2](../images/image2.jpg){ width="50%" }

---

## 📐 Image Alignment

### Left Alignment
```markdown
![Alt text](../images/image1.png){ align=left }
```
**Live Example:**
![Sample Image 1](../images/image1.png){ align=left }
*Text will wrap around the image on the right side. This is a longer paragraph to demonstrate how text flows around the left-aligned image. The image stays on the left while the text continues to flow naturally around it.*

### Right Alignment
```markdown
![Alt text](../images/image2.jpg){ align=right }
```
**Live Example:**
![Sample Image 2](../images/image2.jpg){ align=right }
*Text will wrap around the image on the left side. This demonstrates how right-aligned images work with text flowing around them. The image stays anchored to the right while text flows naturally on the left side of the image.*

---

## 📝 Image Captions

### Method 1: HTML Figure Tags
```markdown
<figure markdown="span">
  ![Alt text](../images/image1.png){ width="300" }
  <figcaption>This is your image caption</figcaption>
</figure>
```

**Live Example:**
<figure markdown="span">
  ![Sample Image 1](../images/image1.png){ width="300" }
  <figcaption>This is a sample caption for the first image</figcaption>
</figure>

### Method 2: Caption Extension
```markdown
![Alt text](../images/image2.jpg){ width="300" }
/// caption
This is your image caption
///
```

**Live Example:**
![Sample Image 2](../images/image2.jpg){ width="300" }
/// caption
This is a sample caption using the extension method
///

---

## 🔍 Lightbox Features

### Basic Lightbox
```markdown
![Alt text](../images/image1.png)
```
*Click any image to open in lightbox mode*

**Live Example:**
![Sample Image 1](../images/image1.png)
*Click the image above to see the lightbox effect!*

### Lightbox with Custom Title
```markdown
![Alt text](../images/image2.jpg "Custom lightbox title")
```

**Live Example:**
![Sample Image 2](../images/image2.jpg "This is a custom lightbox title")
*Click the image above to see the lightbox with custom title!*

---

## ⚡ Performance Features

### Lazy Loading
```markdown
![Alt text](../images/image1.png){ loading=lazy }
```
*Images load only when they come into view*

**Live Example:**
![Sample Image 1](../images/image1.png){ loading=lazy }
*This image uses lazy loading - it will only load when you scroll to it*

### Combined Attributes
```markdown
![Alt text](../images/image2.jpg){ width="300" loading=lazy align=left }
```

**Live Example:**
![Sample Image 2](../images/image2.jpg){ width="300" loading=lazy align=left }
*This image combines multiple attributes: width control, lazy loading, and left alignment. The text flows around it naturally while the image loads only when needed.*

---

## 🌓 Light/Dark Mode Images

### Different Images for Themes
```markdown
![Light mode image](../images/light-screenshot.png#only-light)
![Dark mode image](../images/dark-screenshot.png#only-dark)
```

### Single Image with Theme-Specific Styling
```markdown
![Responsive image](../images/screenshot.png)
```

---

## 🎨 Advanced Examples

### Image Gallery
```markdown
<div class="grid" markdown>

![Image 1](../images/image1.png){ width="200" }
![Image 2](../images/image2.jpg){ width="200" }

</div>
```

**Live Example:**
<div class="grid" markdown>

![Sample Image 1](../images/image1.png){ width="200" }
![Sample Image 2](../images/image2.jpg){ width="200" }

</div>

### Image with Callout
```markdown
!!! info "Sample Image"
    ![Important image](../images/image1.png){ width="400" }
    
    This shows the main configuration screen.
```

**Live Example:**
!!! info "Sample Image"
    ![Important image](../images/image1.png){ width="400" }
    
    This shows the main configuration screen.

### Responsive Image with Caption
```markdown
<figure markdown="span">
  ![Responsive image](../images/image2.jpg){ width="100%" }
  <figcaption>Full-width responsive image with caption</figcaption>
</figure>
```

**Live Example:**
<figure markdown="span">
  ![Responsive image](../images/image2.jpg){ width="100%" }
  <figcaption>Full-width responsive image with caption</figcaption>
</figure>

---

## 🛠️ Troubleshooting

### Common Issues

**Image not showing:**
- Check file path: `../images/filename.jpg`
- Ensure file exists in `docs/../images/`
- Verify file extension is correct

**Lightbox not working:**
- Ensure `mkdocs-glightbox` plugin is installed
- Check `mkdocs.yml` has `glightbox` in plugins section

**Alignment not working:**
- Verify `attr_list` extension is enabled
- Check syntax: `{ align=left }` (note the spaces)

---

## 📋 Quick Reference

| Feature | Syntax | Notes |
|---------|--------|-------|
| **Basic** | `![alt](../images/file.jpg)` | Standard markdown |
| **Width** | `{ width="300" }` | Pixels or percentage |
| **Align Left** | `{ align=left }` | Text wraps right |
| **Align Right** | `{ align=right }` | Text wraps left |
| **Lazy Load** | `{ loading=lazy }` | Performance boost |
| **Caption** | `<figcaption>text</figcaption>` | HTML method |
| **Lightbox** | Automatic | Click to zoom |
| **Light Mode** | `#only-light` | URL fragment |
| **Dark Mode** | `#only-dark` | URL fragment |

---

## 🎯 Best Practices

### ✅ Do
- Use descriptive alt text
- Optimize image file sizes
- Use appropriate formats (PNG for screenshots, JPG for photos)
- Add captions for context
- Use lazy loading for performance

### ❌ Don't
- Use images for text content
- Forget alt text for accessibility
- Use overly large file sizes
- Mix too many alignment styles on one page

---

## 🔗 Related Documentation

- [MkDocs Material Images](https://squidfunk.github.io/mkdocs-material/reference/../images/)
- [Glightbox Plugin](https://github.com/Blueswen/mkdocs-glightbox)
- [Markdown Image Syntax](https://www.markdownguide.org/basic-syntax/#images)

---

*This cheatsheet is automatically updated with the latest MkDocs Material features.*
