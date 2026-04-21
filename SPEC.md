# Image Editor - Specification Document

## 1. Project Overview

- **Project Name**: Simple Image Editor
- **Type**: Django Web Application
- **Core Functionality**: Real-time image processing with convolution-based filters (blur, sharpen, edge detection) and pixel manipulation (brightness, contrast, grayscale, color filters, masking)
- **Target Users**: Users wanting to edit images through an intuitive web interface

---

## 2. UI/UX Specification

### Layout Structure

- **Header**: Project title "Image Editor" with dark background
- **Main Content**: Two-column layout
  - Left: Image display area (canvas with uploaded/processed image)
  - Right: Control panel with sliders and buttons
- **Footer**: Minimal footer with credits

### Responsive Breakpoints
- Desktop: Two-column layout (60% image, 40% controls)
- Tablet/Mobile (<900px): Stacked single column

### Visual Design

**Color Palette**:
- Background: `#0a0a0f` (deep charcoal)
- Panel Background: `#14141f` (dark navy)
- Accent Primary: `#6366f1` (indigo)
- Accent Secondary: `#22d3ee` (cyan)
- Text Primary: `#f1f5f9` (off-white)
- Text Secondary: `#94a3b8` (slate)
- Border/Divider: `#2d2d3f` (muted purple-gray)
- Slider Track: `#3f3f5a` (gray-purple)
- Slider Fill: linear-gradient `#6366f1` to `#22d3ee`

**Typography**:
- Font Family: `"JetBrains Mono", "Fira Code", monospace` for headings/controls
- Font Family: `"Inter", system-ui, sans-serif` for body text
- Title: 28px, weight 700
- Section Headers: 14px, weight 600, uppercase, letter-spacing 2px
- Labels: 13px, weight 500
- Values: 13px, monospace

**Spacing System**:
- Panel padding: 24px
- Section gap: 20px
- Control gap: 12px
- Border radius: 8px (panels), 4px (inputs)

**Visual Effects**:
- Panel shadow: `0 8px 32px rgba(0,0,0,0.4)`
- Subtle glow on active controls: `box-shadow: 0 0 20px rgba(99,102,241,0.3)`
- Smooth transitions: 200ms ease

### Components

**Image Canvas**:
- Max-width: 100% of container
- Centered with checkerboard background (for transparency)
- Border: 1px solid `#2d2d3f`
- Border-radius: 8px

**Sliders**:
- Custom styled range inputs
- Label on left, value display on right
- Slider track height: 6px, border-radius: 3px
- Thumb: 18px circle, gradient fill, shadow
- States: hover (glow), active (larger glow)

**Buttons**:
- Grayscale button: Full-width, gradient border, transparent fill
- Reset button: Secondary style, smaller
- Upload button: Primary gradient fill

**Filter Sections** (in control panel):
1. **Upload Section**: File input + upload button
2. **Convolution Filters**: Blur, Sharpen, Edge Detection sliders (0-10 intensity)
3. **Pixel Adjustments**: Brightness (-100 to 100), Contrast (-100 to 100)
4. **Color Filters**: Grayscale button, Color filter dropdown (Sepia, Vintage, Cool, Warm)
5. **Overlay/Masking**: Second image upload for mask/overlay + blend mode selector
6. **Actions**: Reset All button

---

## 3. Functionality Specification

### Core Features

**Image Upload**:
- Accept JPG, PNG, WebP
- Display original and processed side-by-side or replace
- Store in memory (not disk persistence needed)

**Convolution Filters** (using kernels):

1. **Box Blur** (3x3 kernel):
   ```
   [1,1,1]     scaled by 1/9
   [1,1,1]
   [1,1,1]
   ```
   - Intensity 0-10 scales kernel application iterations

2. **Gaussian Blur** (5x5 kernel approximation):
   ```
   [1,4,6,4,1]   scaled by 1/256
   [4,16,24,16,4]
   [6,24,36,24,6]
   [4,16,24,16,4]
   [1,4,6,4,1]
   ```

3. **Sharpen** (3x3 kernel):
   ```
   [0,-1,0]
   [-1,5,-1]
   [0,-1,0]
   ```

4. **Edge Detection** (Laplacian 3x3):
   ```
   [0,-1,0]
   [-1,4,-1]
   [0,-1,0]
   ```

5. **Sobel Edge** (horizontal and vertical):
   ```
   Gx: [-1,0,1]    Gy: [-1,-2,-1]
       [-2,0,2]        [0,0,0]
       [-1,0,1]        [1,2,1]
   ```

**Pixel Manipulation**:

1. **Brightness**: Add/subtract value to all pixels
   - Formula: `pixel = clamp(pixel + brightness_value)`

2. **Contrast**: Pivot-based contrast adjustment
   - Formula: `pixel = clamp(((pixel - 128) * contrast_factor) + 128)`

3. **Grayscale**: Luminance-weighted average
   - Formula: `gray = 0.299*R + 0.587*G + 0.114*B`
   - Apply to all channels

4. **Color Filters**:
   - **Sepia**: Matrix transformation creating warm brownish tone
   - **Vintage**: Reduced saturation + warm tint
   - **Cool**: Blue channel boost, red reduction
   - **Warm**: Red/orange boost, blue reduction

**Image Masking/Overlay**:
- Upload secondary image
- Blend modes: Multiply, Screen, Overlay
- Multiply: `result = (img1 * img2) / 255`
- Screen: `result = 255 - (255-img1)*(255-img2)/255`
- Overlay: Combination of multiply and screen based on base pixel

**Reset Functionality**:
- Reset button restores original uploaded image
- All sliders return to 0/default

### User Interactions and Flows

1. User uploads image via file input
2. Image displays in canvas area
3. User adjusts sliders → real-time preview
4. User clicks Grayscale → instant conversion
5. User selects color filter from dropdown
6. User optionally uploads second image for overlay
7. User clicks Reset to restore original

### Edge Cases

- No image uploaded: Disable all controls, show placeholder
- Very large images: Resize for display, process at original resolution
- Second image different size: Resize second to match first
- Invalid file type: Show error message
- Slider at 0: No effect applied

---

## 4. Technical Architecture

### Backend (Django)

**Endpoints**:
- `GET /` - Render main page
- `POST /upload/` - Handle image upload, return base64 encoded image
- `POST /process/` - Receive parameters, apply filters, return processed image

**Data Handling**:
- Images as base64 strings for JSON serialization
- Numpy arrays for pixel manipulation
- OpenCV for image loading and conversion

### Frontend

- Vanilla JavaScript for interactivity
- Canvas API for image display
- Fetch API for backend communication
- Real-time slider updates with debouncing

### Libraries

- Django 4.x
- numpy
- opencv-python (cv2)
- Pillow (for additional image formats)

---

## 5. Acceptance Criteria

- [ ] User can upload JPG/PNG images
- [ ] Blur slider applies box blur with adjustable intensity
- [ ] Sharpen slider applies sharpen filter with adjustable intensity
- [ ] Edge detection slider applies edge detection with adjustable intensity
- [ ] Brightness slider adjusts image brightness (-100 to 100)
- [ ] Contrast slider adjusts image contrast (-100 to 100)
- [ ] Grayscale button converts image to grayscale instantly
- [ ] Color filter dropdown applies sepia/vintage/cool/warm filters
- [ ] Second image upload enables overlay/blend functionality
- [ ] Reset button restores original image
- [ ] UI is responsive and visually polished
- [ ] No console errors during operation
- [ ] HD images load and process without freezing UI
