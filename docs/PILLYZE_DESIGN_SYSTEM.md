# Pillyze Design System Analysis

**Analysis Date**: 2024-12-23
**Total Screenshots Analyzed**: 129 screens
**Purpose**: Extract design system for Lottomate app implementation

---

## Executive Summary

Pillyze demonstrates a modern, vibrant health & wellness app design with strong emphasis on:
- **Bold primary purple color (#7C3AED / #6D28D9)** used consistently for brand identity
- **Playful 3D illustrations and emoji-based visual language** for engagement
- **Card-based layouts** with generous white space and rounded corners
- **Clear information hierarchy** with large headings and structured content sections
- **Warm, friendly tone** through illustrations and micro-interactions
- **Accessibility-focused** with high contrast ratios and clear tap targets

The design is highly applicable to Lottomate, which shares similar characteristics (consumer-facing, needs to be trustworthy yet approachable, data-driven).

---

## 1. Color System

### Primary Colors

#### Purple (Brand Primary)
- **Primary Purple**: `#7C3AED` (rgb 124, 58, 237)
  - Main CTA buttons, active states, selected items
  - Bottom navigation active icons
  - Links and interactive elements
  - Splash screen background (full screen purple gradient)

- **Purple Dark**: `#6D28D9` (rgb 109, 40, 217)
  - Button hover/pressed states
  - Darker purple for depth in gradients

- **Purple Light**: `#E9D5FF` / `#F3E8FF` (rgb 233, 213, 255)
  - Light purple backgrounds for cards
  - Selected state backgrounds
  - Checkbox/radio button backgrounds when selected

- **Purple Tint**: `#DDD6FE` (rgb 221, 214, 254)
  - Subtle purple tints for intake time cards
  - Pill-shaped tag backgrounds

### Secondary Colors

#### Status Colors (Semantic)
- **Warning (Yellow/Orange)**: `#FCD34D` (rgb 252, 211, 77) → "주의" badges
- **Danger (Pink/Red)**: `#FDA4AF` (rgb 253, 164, 175) → "부족" badges
- **Success (Cyan)**: `#67E8F9` (rgb 103, 232, 249) → "최적" badges
- **Info (Mint Green)**: `#6EE7B7` (rgb 110, 231, 183) → "최소" badges

#### Accent Colors
- **Kakao Yellow**: `#FEE500` (rgb 254, 229, 0) - Kakao login button
- **Apple Black**: `#000000` - Apple login button
- **Blue Accent**: `#3B82F6` (rgb 59, 130, 246) - Time badges, links
- **Sky Blue**: `#7DD3FC` (rgb 125, 211, 252) - Chart accents

### Neutral Colors

#### Grays (Background & Text)
- **Background White**: `#FFFFFF`
- **Background Gray**: `#F9FAFB` (rgb 249, 250, 251) - Page backgrounds
- **Surface Gray**: `#F3F4F6` (rgb 243, 244, 246) - Card backgrounds, input fields
- **Border Gray**: `#E5E7EB` (rgb 229, 231, 235) - Dividers, borders
- **Text Primary**: `#111827` (rgb 17, 24, 39) - Headings, primary text
- **Text Secondary**: `#6B7280` (rgb 107, 114, 128) - Body text, labels
- **Text Tertiary**: `#9CA3AF` (rgb 156, 163, 175) - Placeholder text, disabled text
- **Icon Gray**: `#D1D5DB` (rgb 209, 213, 219) - Inactive icons

### Gradients

#### Purple Gradient (Splash/Hero)
```css
linear-gradient(180deg, #8B5CF6 0%, #7C3AED 100%)
```

#### Emoji/Illustration Gradients
- Yellow gradient: `#FBBF24 → #F59E0B` (happy/good state)
- Orange gradient: `#FB923C → #F97316` (neutral state)
- Pink gradient: `#F472B6 → #EC4899` (bad state)

### Color Usage Guidelines

**For Lottomate Application:**
1. **Primary Actions**: Use Purple `#7C3AED` for "번호 저장", "당첨 확인" buttons
2. **Status Badges**:
   - "1등" → Success (Cyan `#67E8F9`)
   - "2등, 3등" → Info (Mint `#6EE7B7`)
   - "꽝" → Neutral (Gray)
   - "미추첨" → Warning (Yellow `#FCD34D`)
3. **Number Balls**: Consider gradient fills like Pillyze's emoji system
4. **Background**: Use `#F9FAFB` for main background, white cards

---

## 2. Typography System

### Font Family
- **Primary**: System default (San Francisco on iOS, Roboto on Android)
- **Korean**: Noto Sans KR / Apple SD Gothic Neo
- **No custom fonts** - relies on platform defaults for best performance

### Type Scale

#### Display Styles (Large Titles)
```kotlin
// Display Large - Onboarding headlines
fontSize = 28.sp
fontWeight = FontWeight.Bold
lineHeight = 36.sp
letterSpacing = (-0.5).sp

// Display Medium - Screen titles
fontSize = 24.sp
fontWeight = FontWeight.Bold
lineHeight = 32.sp
letterSpacing = (-0.3).sp
```

#### Heading Styles
```kotlin
// Heading 1 - Section titles
fontSize = 20.sp
fontWeight = FontWeight.Bold
lineHeight = 28.sp

// Heading 2 - Subsection titles
fontSize = 18.sp
fontWeight = FontWeight.SemiBold
lineHeight = 24.sp

// Heading 3 - Card titles
fontSize = 16.sp
fontWeight = FontWeight.SemiBold
lineHeight = 22.sp
```

#### Body Styles
```kotlin
// Body Large - Primary content
fontSize = 16.sp
fontWeight = FontWeight.Normal
lineHeight = 24.sp

// Body Medium - Default body text
fontSize = 14.sp
fontWeight = FontWeight.Normal
lineHeight = 20.sp

// Body Small - Secondary text
fontSize = 13.sp
fontWeight = FontWeight.Normal
lineHeight = 18.sp
```

#### Label Styles
```kotlin
// Label Large - Button text
fontSize = 16.sp
fontWeight = FontWeight.SemiBold
lineHeight = 20.sp

// Label Medium - Input labels
fontSize = 14.sp
fontWeight = FontWeight.Medium
lineHeight = 18.sp

// Label Small - Captions, metadata
fontSize = 12.sp
fontWeight = FontWeight.Medium
lineHeight = 16.sp
```

#### Caption Styles
```kotlin
// Caption - Timestamps, hints
fontSize = 11.sp
fontWeight = FontWeight.Normal
lineHeight = 14.sp
color = TextSecondary (#6B7280)
```

### Text Colors by Context
- **Headings**: `#111827` (Text Primary)
- **Body Text**: `#374151` (Text Primary, slightly lighter)
- **Secondary Text**: `#6B7280` (Text Secondary)
- **Placeholder**: `#9CA3AF` (Text Tertiary)
- **Disabled**: `#D1D5DB` (Icon Gray)
- **Links**: `#7C3AED` (Primary Purple)
- **Error**: `#EF4444` (Red)

### Typography Usage Examples

```kotlin
// Screen Title
Text(
    text = "번호 추첨 결과",
    style = MaterialTheme.typography.displayMedium,
    color = MaterialTheme.colorScheme.onBackground
)

// Section Header
Text(
    text = "나의 번호",
    style = MaterialTheme.typography.headlineSmall,
    fontWeight = FontWeight.Bold
)

// Body Content
Text(
    text = "1등 당첨번호와 일치하는 번호가 있습니다",
    style = MaterialTheme.typography.bodyLarge,
    color = MaterialTheme.colorScheme.onSurfaceVariant
)

// Metadata
Text(
    text = "1134회차 • 2024.12.23",
    style = MaterialTheme.typography.labelSmall,
    color = MaterialTheme.colorScheme.outline
)
```

---

## 3. Spacing & Layout System

### Spacing Scale (8pt Grid)
```kotlin
object Spacing {
    val xxxs = 2.dp   // Tight spacing
    val xxs = 4.dp    // Very small spacing
    val xs = 8.dp     // Small spacing
    val sm = 12.dp    // Small-medium spacing
    val md = 16.dp    // Medium spacing (most common)
    val lg = 20.dp    // Large spacing
    val xl = 24.dp    // Extra large spacing
    val xxl = 32.dp   // Section spacing
    val xxxl = 40.dp  // Major section spacing
    val huge = 48.dp  // Hero spacing
}
```

### Common Patterns

#### Screen Padding
- **Horizontal padding**: `16.dp` (universal for most screens)
- **Top padding**: `16.dp` from status bar/app bar
- **Bottom padding**: `16.dp` above bottom navigation
- **Content padding**: `20.dp` for prominent sections

#### Card Spacing
- **Card padding**: `16.dp` internal padding
- **Card margin**: `16.dp` horizontal, `12.dp` vertical between cards
- **Card corner radius**: `16.dp` (large, friendly)

#### List Item Spacing
- **Item height**: `56.dp` minimum (touch target)
- **Item padding**: `16.dp` horizontal, `12.dp` vertical
- **Icon spacing**: `12.dp` between icon and text
- **Between items**: `8.dp` for tight lists, `12.dp` for loose lists

#### Button Spacing
- **Button height**: `48.dp` (default), `56.dp` (prominent)
- **Button padding**: `16.dp` horizontal, `12.dp` vertical
- **Between buttons**: `12.dp` for stacked buttons
- **Button to content**: `24.dp` separation

#### Section Spacing
- **Between sections**: `32.dp` (clear visual separation)
- **Section title to content**: `16.dp`
- **Within section**: `12.dp` between related items

### Layout Patterns

#### Screen Layout Structure
```
┌─────────────────────────────┐
│ Status Bar                  │
├─────────────────────────────┤
│ App Bar (56.dp)             │  ← 16.dp padding horizontal
├─────────────────────────────┤
│                             │
│ Content Area                │  ← 16.dp padding horizontal
│ - Cards with 16.dp radius   │  ← 12.dp spacing between cards
│ - Sections with 32.dp gap   │
│                             │
├─────────────────────────────┤
│ Bottom Navigation (56.dp)   │
├─────────────────────────────┤
│ System Navigation Bar       │
└─────────────────────────────┘
```

#### Card Layout (Common Pattern)
```
Card (16.dp corner radius, 16.dp padding) {
    Column(spacing = 12.dp) {
        Heading (headlineSmall)
        Body Text (bodyMedium, +8.dp top)
        Divider (if multi-section, +16.dp top, +16.dp bottom)
        Action Area (buttons/links, +8.dp top)
    }
}
```

---

## 4. Component Library

### 4.1 Buttons

#### Primary Button
```kotlin
Button(
    onClick = { },
    modifier = Modifier
        .fillMaxWidth()
        .height(56.dp),
    shape = RoundedCornerShape(16.dp),
    colors = ButtonDefaults.buttonColors(
        containerColor = Color(0xFF7C3AED), // Purple
        contentColor = Color.White
    )
) {
    Text(
        text = "확인",
        style = MaterialTheme.typography.labelLarge,
        fontWeight = FontWeight.SemiBold
    )
}
```

**Properties:**
- Height: `56.dp` (prominent actions), `48.dp` (secondary)
- Corner radius: `16.dp`
- Background: Purple `#7C3AED`
- Text: White, `16.sp`, SemiBold
- Full width or minimum `120.dp`
- Padding: `16.dp` horizontal

**States:**
- Normal: Purple `#7C3AED`
- Pressed: Darker purple `#6D28D9`
- Disabled: Gray `#E5E7EB`, text gray `#9CA3AF`

#### Secondary Button (Outlined)
```kotlin
OutlinedButton(
    onClick = { },
    modifier = Modifier
        .fillMaxWidth()
        .height(48.dp),
    shape = RoundedCornerShape(16.dp),
    border = BorderStroke(1.dp, Color(0xFF7C3AED)),
    colors = ButtonDefaults.outlinedButtonColors(
        contentColor = Color(0xFF7C3AED)
    )
) {
    Text("다음에 할게요", style = MaterialTheme.typography.labelMedium)
}
```

**Properties:**
- Height: `48.dp`
- Border: `1.dp` purple
- Background: Transparent/White
- Text: Purple `#7C3AED`

#### Text Button
```kotlin
TextButton(onClick = { }) {
    Text(
        text = "자세히 보기",
        color = Color(0xFF6B7280),
        style = MaterialTheme.typography.labelMedium
    )
}
```

**Properties:**
- No background, no border
- Text: Gray `#6B7280`
- Underline on press (optional)

#### Social Login Buttons
**Kakao:**
```kotlin
Button(
    onClick = { },
    colors = ButtonDefaults.buttonColors(
        containerColor = Color(0xFFFEE500),
        contentColor = Color(0xFF000000)
    ),
    modifier = Modifier.fillMaxWidth().height(56.dp),
    shape = RoundedCornerShape(16.dp)
) {
    Icon(painter = painterResource(R.drawable.ic_kakao), contentDescription = null)
    Spacer(Modifier.width(8.dp))
    Text("카카오로 3초만에 로그인", fontWeight = FontWeight.SemiBold)
}
```

**Apple:**
```kotlin
Button(
    onClick = { },
    colors = ButtonDefaults.buttonColors(
        containerColor = Color(0xFF000000),
        contentColor = Color.White
    ),
    modifier = Modifier.fillMaxWidth().height(56.dp),
    shape = RoundedCornerShape(16.dp)
) {
    Icon(painter = painterResource(R.drawable.ic_apple), contentColor = Color.White)
    Spacer(Modifier.width(8.dp))
    Text("Apple로 로그인", fontWeight = FontWeight.SemiBold)
}
```

### 4.2 Cards

#### Basic Card
```kotlin
Card(
    modifier = Modifier.fillMaxWidth(),
    shape = RoundedCornerShape(16.dp),
    colors = CardDefaults.cardColors(
        containerColor = Color.White
    ),
    elevation = CardDefaults.cardElevation(
        defaultElevation = 0.dp // Flat design, no shadow
    )
) {
    Column(
        modifier = Modifier.padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(12.dp)
    ) {
        Text("Card Title", style = MaterialTheme.typography.headlineSmall)
        Text("Card content", style = MaterialTheme.typography.bodyMedium)
    }
}
```

**Properties:**
- Corner radius: `16.dp`
- Background: White `#FFFFFF`
- Border: None (or 1.dp gray `#E5E7EB` for emphasis)
- Padding: `16.dp`
- Elevation: `0.dp` (flat design)
- Spacing between cards: `12.dp`

#### Card with Icon/Emoji
```kotlin
Card(
    modifier = Modifier.fillMaxWidth(),
    shape = RoundedCornerShape(16.dp),
    colors = CardDefaults.cardColors(containerColor = Color.White)
) {
    Row(
        modifier = Modifier.padding(16.dp),
        verticalAlignment = Alignment.CenterVertically,
        horizontalArrangement = Arrangement.spacedBy(12.dp)
    ) {
        // Emoji or icon
        Text("💊", fontSize = 32.sp)

        Column(modifier = Modifier.weight(1f)) {
            Text("멜라토닌", style = MaterialTheme.typography.headlineSmall)
            Text("1정", style = MaterialTheme.typography.bodySmall, color = Color(0xFF6B7280))
        }

        // Trailing badge
        Surface(
            shape = RoundedCornerShape(8.dp),
            color = Color(0xFFE9D5FF)
        ) {
            Text(
                text = "2캡슐",
                modifier = Modifier.padding(horizontal = 8.dp, vertical = 4.dp),
                color = Color(0xFF7C3AED),
                style = MaterialTheme.typography.labelSmall
            )
        }
    }
}
```

#### Analysis Result Card (Purple Background)
```kotlin
Card(
    modifier = Modifier.fillMaxWidth(),
    shape = RoundedCornerShape(20.dp),
    colors = CardDefaults.cardColors(
        containerColor = Color(0xFFF3E8FF) // Light purple
    )
) {
    Column(
        modifier = Modifier.padding(20.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        // Grid of badges (부족, 주의, 최적, 최소)
        Row(
            horizontalArrangement = Arrangement.spacedBy(8.dp)
        ) {
            BadgeChip("부족", count = "6개", color = Color(0xFFFDA4AF))
            BadgeChip("주의", count = "4개", color = Color(0xFFFCD34D))
        }
        Row(
            horizontalArrangement = Arrangement.spacedBy(8.dp)
        ) {
            BadgeChip("최적", count = "2개", color = Color(0xFF67E8F9))
            BadgeChip("최소", count = "12개", color = Color(0xFF6EE7B7))
        }
    }
}
```

### 4.3 Input Fields

#### Text Input Field
```kotlin
OutlinedTextField(
    value = text,
    onValueChange = { text = it },
    modifier = Modifier.fillMaxWidth(),
    placeholder = {
        Text("5글자 내로 입력해주세요", color = Color(0xFF9CA3AF))
    },
    shape = RoundedCornerShape(12.dp),
    colors = OutlinedTextFieldDefaults.colors(
        focusedBorderColor = Color(0xFF7C3AED),
        unfocusedBorderColor = Color(0xFFE5E7EB),
        focusedContainerColor = Color.White,
        unfocusedContainerColor = Color(0xFFF9FAFB)
    ),
    textStyle = MaterialTheme.typography.bodyLarge
)
```

**Properties:**
- Height: `56.dp` (default)
- Corner radius: `12.dp`
- Border: `1.dp` gray → purple on focus
- Background: Light gray `#F9FAFB` → white on focus
- Padding: `16.dp` horizontal, `14.dp` vertical
- Placeholder: Gray `#9CA3AF`

**Label Position:**
- External label (above field): `labelMedium`, 8.dp spacing
- Floating label: Not used in Pillyze

**Character Counter:**
```kotlin
Text(
    text = "0/5",
    modifier = Modifier.align(Alignment.End).padding(top = 4.dp),
    style = MaterialTheme.typography.labelSmall,
    color = Color(0xFF9CA3AF)
)
```

#### Search Input
```kotlin
OutlinedTextField(
    value = searchQuery,
    onValueChange = { searchQuery = it },
    modifier = Modifier.fillMaxWidth(),
    placeholder = { Text("제품명, 브랜드명, 증상으로 검색") },
    leadingIcon = {
        Icon(
            imageVector = Icons.Default.Search,
            contentDescription = "Search",
            tint = Color(0xFF9CA3AF)
        )
    },
    trailingIcon = {
        if (searchQuery.isNotEmpty()) {
            IconButton(onClick = { searchQuery = "" }) {
                Icon(Icons.Default.Close, contentDescription = "Clear")
            }
        }
    },
    shape = RoundedCornerShape(24.dp),
    colors = OutlinedTextFieldDefaults.colors(
        focusedBorderColor = Color(0xFF7C3AED),
        unfocusedBorderColor = Color.Transparent,
        focusedContainerColor = Color.White,
        unfocusedContainerColor = Color(0xFFF3F4F6)
    )
)
```

**Properties:**
- Fully rounded: `24.dp` corner radius
- Height: `48.dp`
- Leading icon: Search icon (gray)
- Trailing icon: Clear button (X) when text present
- Background: Light gray, no border when unfocused

### 4.4 Chips & Badges

#### Selection Chip (Pill)
```kotlin
Surface(
    onClick = { selected = !selected },
    shape = RoundedCornerShape(20.dp),
    color = if (selected) Color(0xFF7C3AED) else Color.Transparent,
    border = BorderStroke(
        width = 1.dp,
        color = if (selected) Color(0xFF7C3AED) else Color(0xFFE5E7EB)
    )
) {
    Row(
        modifier = Modifier.padding(horizontal = 16.dp, vertical = 8.dp),
        verticalAlignment = Alignment.CenterVertically,
        horizontalArrangement = Arrangement.spacedBy(4.dp)
    ) {
        if (selected) {
            Icon(
                imageVector = Icons.Default.Check,
                contentDescription = null,
                tint = Color.White,
                modifier = Modifier.size(16.dp)
            )
        }
        Text(
            text = "멜라토닌",
            color = if (selected) Color.White else Color(0xFF374151),
            style = MaterialTheme.typography.labelMedium
        )
    }
}
```

**Properties:**
- Corner radius: `20.dp` (fully rounded)
- Height: `32.dp` (auto from padding)
- Padding: `16.dp` horizontal, `8.dp` vertical
- Border: `1.dp` gray → purple when selected
- Background: Transparent → purple when selected
- Text: Gray → white when selected
- Checkmark icon appears when selected

#### Status Badge (Tag)
```kotlin
@Composable
fun StatusBadge(
    text: String,
    type: BadgeType // EXCESS, WARNING, OPTIMAL, MINIMAL
) {
    val (bgColor, textColor) = when (type) {
        BadgeType.EXCESS -> Color(0xFFFCE7F3) to Color(0xFFF472B6) // Pink
        BadgeType.WARNING -> Color(0xFFFEF3C7) to Color(0xFFF59E0B) // Yellow
        BadgeType.OPTIMAL -> Color(0xFFCCFBF1) to Color(0xFF06B6D4) // Cyan
        BadgeType.MINIMAL -> Color(0xFFD1FAE5) to Color(0xFF10B981) // Green
    }

    Surface(
        shape = RoundedCornerShape(6.dp),
        color = bgColor
    ) {
        Text(
            text = text,
            modifier = Modifier.padding(horizontal = 8.dp, vertical = 4.dp),
            color = textColor,
            style = MaterialTheme.typography.labelSmall,
            fontWeight = FontWeight.SemiBold
        )
    }
}

// Usage
StatusBadge("부족", BadgeType.EXCESS)
StatusBadge("주의", BadgeType.WARNING)
StatusBadge("최적", BadgeType.OPTIMAL)
StatusBadge("최소", BadgeType.MINIMAL)
```

**Properties:**
- Corner radius: `6.dp` (small, subtle)
- Padding: `8.dp` horizontal, `4.dp` vertical
- Background: Light tint of status color
- Text: Bold status color, `12.sp`

#### Removable Chip (Filter Chip)
```kotlin
Surface(
    shape = RoundedCornerShape(20.dp),
    color = Color(0xFF7C3AED)
) {
    Row(
        modifier = Modifier.padding(start = 16.dp, end = 8.dp, top = 8.dp, bottom = 8.dp),
        verticalAlignment = Alignment.CenterVertically,
        horizontalArrangement = Arrangement.spacedBy(4.dp)
    ) {
        Text(
            text = "비타민B1",
            color = Color.White,
            style = MaterialTheme.typography.labelMedium
        )
        IconButton(
            onClick = { /* Remove */ },
            modifier = Modifier.size(20.dp)
        ) {
            Icon(
                imageVector = Icons.Default.Close,
                contentDescription = "Remove",
                tint = Color.White,
                modifier = Modifier.size(16.dp)
            )
        }
    }
}
```

**Properties:**
- Same as selection chip, but with X button
- X button: `20.dp` touch target, `16.dp` icon

### 4.5 Lists & List Items

#### Simple List Item
```kotlin
Row(
    modifier = Modifier
        .fillMaxWidth()
        .clickable { }
        .padding(horizontal = 16.dp, vertical = 12.dp),
    verticalAlignment = Alignment.CenterVertically,
    horizontalArrangement = Arrangement.spacedBy(12.dp)
) {
    // Leading icon/emoji
    Text("💊", fontSize = 24.sp)

    // Content
    Column(modifier = Modifier.weight(1f)) {
        Text(
            text = "멜라토닌",
            style = MaterialTheme.typography.bodyLarge,
            fontWeight = FontWeight.Medium
        )
        Text(
            text = "1일 1회 1정",
            style = MaterialTheme.typography.bodySmall,
            color = Color(0xFF6B7280)
        )
    }

    // Trailing icon
    Icon(
        imageVector = Icons.Default.ChevronRight,
        contentDescription = null,
        tint = Color(0xFF9CA3AF)
    )
}
```

**Properties:**
- Min height: `56.dp` (touch target)
- Padding: `16.dp` horizontal, `12.dp` vertical
- Icon size: `24.dp` (emoji larger ~32sp)
- Icon-to-text spacing: `12.dp`
- Trailing chevron: Gray `#9CA3AF`

#### List Item with Checkbox
```kotlin
Row(
    modifier = Modifier
        .fillMaxWidth()
        .toggleable(
            value = checked,
            onValueChange = { checked = it }
        )
        .padding(16.dp),
    verticalAlignment = Alignment.CenterVertically
) {
    Checkbox(
        checked = checked,
        onCheckedChange = null, // Handled by row click
        colors = CheckboxDefaults.colors(
            checkedColor = Color(0xFF7C3AED),
            uncheckedColor = Color(0xFFD1D5DB)
        )
    )
    Spacer(Modifier.width(12.dp))
    Column(modifier = Modifier.weight(1f)) {
        Text("(필수) 서비스 이용약관 동의", style = MaterialTheme.typography.bodyMedium)
        TextButton(onClick = { }, modifier = Modifier.padding(0.dp)) {
            Text("보기", style = MaterialTheme.typography.labelSmall, color = Color(0xFF6B7280))
        }
    }
}
```

#### List with Dividers
```kotlin
Column {
    items.forEachIndexed { index, item ->
        ListItem(item)
        if (index < items.lastIndex) {
            Divider(
                color = Color(0xFFE5E7EB),
                thickness = 1.dp,
                modifier = Modifier.padding(horizontal = 16.dp)
            )
        }
    }
}
```

**Divider Properties:**
- Color: `#E5E7EB` (border gray)
- Thickness: `1.dp`
- Inset: `16.dp` from left (aligned with content)

### 4.6 Bottom Sheets & Dialogs

#### Bottom Sheet
```kotlin
ModalBottomSheet(
    onDismissRequest = { showBottomSheet = false },
    sheetState = rememberModalBottomSheetState(),
    shape = RoundedCornerShape(topStart = 24.dp, topEnd = 24.dp),
    containerColor = Color.White,
    dragHandle = {
        Surface(
            modifier = Modifier
                .padding(top = 12.dp, bottom = 8.dp)
                .width(40.dp)
                .height(4.dp),
            shape = RoundedCornerShape(2.dp),
            color = Color(0xFFD1D5DB)
        ) {}
    }
) {
    Column(
        modifier = Modifier.padding(horizontal = 16.dp, vertical = 24.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        Text(
            text = "하루에 총 몇 캡슐 드세요?",
            style = MaterialTheme.typography.headlineSmall,
            fontWeight = FontWeight.Bold
        )
        Text(
            text = "(예시: 1일 2회 2캡슐 = 4캡슐)",
            style = MaterialTheme.typography.bodyMedium,
            color = Color(0xFF6B7280)
        )

        // Content (e.g., number picker)

        Button(
            onClick = { },
            modifier = Modifier.fillMaxWidth()
        ) {
            Text("확인")
        }
    }
}
```

**Properties:**
- Top corner radius: `24.dp`
- Drag handle: `40.dp` × `4.dp`, rounded, gray
- Padding: `16.dp` horizontal, `24.dp` vertical
- Max height: `90%` of screen (wrap content preferred)

#### Dialog (Popup)
```kotlin
Dialog(onDismissRequest = { showDialog = false }) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .wrapContentHeight(),
        shape = RoundedCornerShape(20.dp),
        colors = CardDefaults.cardColors(containerColor = Color.White)
    ) {
        Column(
            modifier = Modifier.padding(24.dp),
            horizontalAlignment = Alignment.CenterHorizontally,
            verticalArrangement = Arrangement.spacedBy(16.dp)
        ) {
            // Emoji illustration
            Text("😊", fontSize = 64.sp)

            Text(
                text = "필라이즈 서비스에\n만족하시나요?",
                style = MaterialTheme.typography.headlineSmall,
                fontWeight = FontWeight.Bold,
                textAlign = TextAlign.Center
            )

            Text(
                text = "만족하신다니 기쁘어요!\n더 만족스러운 필라이즈가 될 수 있도록\n앱 마켓에도 리뷰를 남겨 주세요.",
                style = MaterialTheme.typography.bodyMedium,
                color = Color(0xFF6B7280),
                textAlign = TextAlign.Center
            )

            Button(
                onClick = { },
                modifier = Modifier.fillMaxWidth()
            ) {
                Text("확인")
            }

            TextButton(onClick = { }) {
                Text("다음에 할게요", color = Color(0xFF9CA3AF))
            }
        }
    }
}
```

**Properties:**
- Corner radius: `20.dp`
- Padding: `24.dp`
- Centered on screen
- Emoji: `64.sp` for main illustration
- Button spacing: `16.dp` between elements

### 4.7 Navigation

#### Bottom Navigation Bar
```kotlin
NavigationBar(
    containerColor = Color.White,
    contentColor = Color(0xFF7C3AED),
    tonalElevation = 0.dp // Flat design
) {
    NavigationBarItem(
        icon = {
            Icon(
                painter = painterResource(R.drawable.ic_home),
                contentDescription = "홈",
                modifier = Modifier.size(24.dp)
            )
        },
        label = {
            Text(
                "홈",
                style = MaterialTheme.typography.labelSmall,
                fontWeight = if (selected) FontWeight.SemiBold else FontWeight.Normal
            )
        },
        selected = selected,
        onClick = { },
        colors = NavigationBarItemDefaults.colors(
            selectedIconColor = Color(0xFF7C3AED),
            selectedTextColor = Color(0xFF7C3AED),
            indicatorColor = Color.Transparent, // No background indicator
            unselectedIconColor = Color(0xFFD1D5DB),
            unselectedTextColor = Color(0xFF9CA3AF)
        )
    )
}
```

**Properties:**
- Height: `56.dp`
- Icon size: `24.dp`
- Label: `labelSmall`, SemiBold when selected
- Selected color: Purple `#7C3AED`
- Unselected color: Gray `#D1D5DB`
- No background indicator (Pillyze style)
- 5 items: 홈, 분석, 추천, 섭취, 건강기록

#### Top App Bar
```kotlin
TopAppBar(
    title = {
        Text(
            "영양제 먹는 시간",
            style = MaterialTheme.typography.titleLarge
        )
    },
    navigationIcon = {
        IconButton(onClick = { }) {
            Icon(
                imageVector = Icons.Default.ArrowBack,
                contentDescription = "Back",
                tint = Color(0xFF111827)
            )
        }
    },
    actions = {
        IconButton(onClick = { }) {
            Icon(
                painter = painterResource(R.drawable.ic_settings),
                contentDescription = "Settings",
                tint = Color(0xFF111827)
            )
        }
    },
    colors = TopAppBarDefaults.topAppBarColors(
        containerColor = Color.White,
        titleContentColor = Color(0xFF111827)
    )
)
```

**Properties:**
- Height: `56.dp`
- Background: White (or purple for branded sections)
- Title: `titleLarge`, centered or left-aligned
- Icons: `24.dp`, black (or white on purple background)
- Elevation: `0.dp` (flat)

**Branded Top Bar (Purple):**
```kotlin
TopAppBar(
    title = {
        Row(verticalAlignment = Alignment.CenterVertically) {
            Text("Pillyze", color = Color.White, fontWeight = FontWeight.Bold)
            Text(" | Q&A", color = Color.White)
        }
    },
    actions = {
        IconButton(onClick = { }) {
            Icon(Icons.Default.Notifications, contentDescription = "알림", tint = Color.White)
        }
        IconButton(onClick = { }) {
            Icon(Icons.Default.Person, contentDescription = "마이페이지", tint = Color.White)
        }
    },
    colors = TopAppBarDefaults.topAppBarColors(
        containerColor = Color(0xFF7C3AED),
        titleContentColor = Color.White
    )
)
```

### 4.8 Empty States

#### Empty State Component
```kotlin
@Composable
fun EmptyState(
    emoji: String = "👻",
    title: String,
    description: String? = null,
    actionButton: (@Composable () -> Unit)? = null
) {
    Column(
        modifier = Modifier
            .fillMaxWidth()
            .padding(48.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        // Ghost emoji with subtle animation
        Text(
            text = emoji,
            fontSize = 80.sp,
            color = Color(0xFFE5E7EB).copy(alpha = 0.6f)
        )

        Text(
            text = title,
            style = MaterialTheme.typography.bodyLarge,
            color = Color(0xFF9CA3AF),
            textAlign = TextAlign.Center
        )

        description?.let {
            Text(
                text = it,
                style = MaterialTheme.typography.bodySmall,
                color = Color(0xFFD1D5DB),
                textAlign = TextAlign.Center
            )
        }

        actionButton?.invoke()
    }
}

// Usage
EmptyState(
    emoji = "👻",
    title = "아직 선택하신 영양제가 없어요",
    description = "영양제를 등록해주세요"
)
```

**Properties:**
- Emoji: `80.sp`, light gray with reduced opacity
- Title: `bodyLarge`, gray `#9CA3AF`
- Description: `bodySmall`, lighter gray `#D1D5DB`
- Centered vertically and horizontally
- Spacing: `16.dp` between elements
- Padding: `48.dp` from edges

### 4.9 Loading States

#### Loading Spinner
```kotlin
@Composable
fun LoadingIndicator(
    text: String = "WWIT님의 영양제를\n분석하고 있어요"
) {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .background(Color.White),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        // Animated illustration (Lottie or custom)
        Image(
            painter = painterResource(R.drawable.img_loading),
            contentDescription = null,
            modifier = Modifier.size(200.dp)
        )

        Spacer(Modifier.height(24.dp))

        CircularProgressIndicator(
            modifier = Modifier.size(40.dp),
            color = Color(0xFF7C3AED),
            strokeWidth = 4.dp
        )

        Spacer(Modifier.height(16.dp))

        Text(
            text = text,
            style = MaterialTheme.typography.bodyLarge,
            fontWeight = FontWeight.Medium,
            textAlign = TextAlign.Center
        )
    }
}
```

**Properties:**
- Full screen overlay
- Illustration: `200.dp` size
- Spinner: `40.dp` diameter, purple, `4.dp` stroke
- Text: Below spinner, centered
- White background

### 4.10 Progress Indicators

#### Step Progress Indicator
```kotlin
@Composable
fun StepProgress(currentStep: Int, totalSteps: Int) {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .padding(horizontal = 16.dp),
        horizontalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        repeat(totalSteps) { index ->
            Box(
                modifier = Modifier
                    .weight(1f)
                    .height(4.dp)
                    .background(
                        color = if (index < currentStep) Color(0xFF7C3AED) else Color(0xFFE5E7EB),
                        shape = RoundedCornerShape(2.dp)
                    )
            )
        }
    }
}
```

**Properties:**
- Height: `4.dp`
- Corner radius: `2.dp`
- Active color: Purple `#7C3AED`
- Inactive color: Gray `#E5E7EB`
- Spacing: `8.dp` between steps

#### Circular Progress (Success)
```kotlin
Box(
    contentAlignment = Alignment.Center,
    modifier = Modifier.size(120.dp)
) {
    CircularProgressIndicator(
        progress = 1f,
        modifier = Modifier.size(120.dp),
        color = Color(0xFF7C3AED),
        strokeWidth = 8.dp,
        strokeCap = StrokeCap.Round
    )
    Icon(
        imageVector = Icons.Default.Check,
        contentDescription = null,
        tint = Color(0xFF7C3AED),
        modifier = Modifier.size(48.dp)
    )
}

Text(
    text = "맞춤 분석이 완료되었어요",
    modifier = Modifier.padding(top = 16.dp),
    style = MaterialTheme.typography.bodyLarge,
    fontWeight = FontWeight.Medium
)
```

---

## 5. Iconography & Illustrations

### Icon Style

**General Principles:**
- **Style**: Rounded, friendly outline icons (not filled)
- **Size**: `24.dp` (standard), `20.dp` (small), `32.dp` (large)
- **Stroke width**: `2.dp` (consistent)
- **Color**: Context-dependent (purple for active, gray for inactive)

**Common Icons:**
- Home: House outline
- Search: Magnifying glass
- Settings: Gear/cog
- Profile: User circle
- Notification: Bell (with badge for unread)
- Calendar: Calendar outline
- Check: Checkmark (circular or plain)
- Close: X (circular or plain)
- Chevron: Right/down arrows for navigation
- Plus: Add items
- Info: Circle with "i"

### Emoji Usage

**Pillyze heavily uses emoji for visual communication:**

#### Health Condition Emoji
- 😊 Green smiley: Very good condition
- 🙂 Yellow smiley: Good condition
- 😐 Orange neutral: Fair condition
- 😟 Light red frown: Poor condition
- 😢 Red sad: Very poor condition

#### Content Emoji
- 💊 Pill: Supplements/medication
- 🔬 Beaker: Analysis/testing
- 📊 Chart: Statistics/data
- 📅 Calendar: Schedule/dates
- ⏰ Alarm: Reminders/time
- 💡 Bulb: Tips/insights
- ❤️ Heart: Favorites/likes
- ⚠️ Warning: Alerts/cautions
- ✨ Sparkles: New/special features
- 🎉 Party: Achievements/success
- 🌙 Moon: Night time
- ☀️ Sun: Day time
- 🍊 Fruit: Vitamins
- 💪 Muscle: Exercise/fitness

**For Lottomate:**
- 🎰 Slot machine: Lottery/gambling
- 🎲 Dice: Random/chance
- 💰 Money bag: Winnings
- ⭐ Star: Lucky numbers
- 🎯 Target: Accuracy/precision
- 📈 Chart: Statistics
- 🏆 Trophy: Winning
- 💎 Gem: Premium/special
- ⚡ Lightning: Quick check
- 🎪 Circus tent: Excitement

### 3D Illustrations

**Pillyze Style:**
- Soft, rounded 3D objects
- Pastel gradients (purple, pink, cyan, yellow)
- Glossy surface effects
- Subtle shadows
- Playful, organic shapes

**Example Illustration Subjects:**
- Pills (capsules, tablets) with gradient fills
- Medicine bottles with rounded caps
- Cartoon characters (blob-like mascots)
- Body parts (eye, stomach, intestine) in cute style
- Charts and graphs in 3D

**Gradient Patterns:**
```kotlin
// Purple gradient (brand)
Brush.linearGradient(
    colors = listOf(Color(0xFF8B5CF6), Color(0xFF7C3AED))
)

// Happy/good state (yellow-orange)
Brush.linearGradient(
    colors = listOf(Color(0xFFFBBF24), Color(0xFFF59E0B))
)

// Warning state (orange-red)
Brush.linearGradient(
    colors = listOf(Color(0xFFFB923C), Color(0xFFF97316))
)

// Neutral state (cyan-blue)
Brush.linearGradient(
    colors = listOf(Color(0xFF67E8F9), Color(0xFF22D3EE))
)
```

**For Lottomate:**
- Lottery balls with number gradients
- Ticket illustrations
- Money/coin stacks
- Trophy/ribbon graphics
- Chart/graph visualizations with playful styling

---

## 6. Animation & Micro-interactions

### Transition Patterns

**Screen Transitions:**
- Fade + slide up for bottom sheets: `300ms` ease-in-out
- Slide horizontal for page navigation: `250ms` ease
- Fade for dialogs: `200ms` ease-in

**Button Interactions:**
```kotlin
Button(
    onClick = { },
    modifier = Modifier
        .fillMaxWidth()
        .scale(scale) // Animated scale
        .alpha(alpha), // Animated alpha
    interactionSource = remember { MutableInteractionSource() }
        .also { interactionSource ->
            LaunchedEffect(interactionSource) {
                interactionSource.interactions.collect { interaction ->
                    when (interaction) {
                        is PressInteraction.Press -> {
                            scale = 0.96f
                        }
                        is PressInteraction.Release,
                        is PressInteraction.Cancel -> {
                            scale = 1f
                        }
                    }
                }
            }
        }
) {
    Text("확인")
}
```

**Properties:**
- Press: Scale down to `96%`, duration `100ms`
- Release: Scale back to `100%`, duration `150ms`
- Ripple effect: Purple tint `#7C3AED` with `20%` opacity

### Loading Animations

**Spinner Rotation:**
```kotlin
val infiniteTransition = rememberInfiniteTransition()
val angle by infiniteTransition.animateFloat(
    initialValue = 0f,
    targetValue = 360f,
    animationSpec = infiniteRepeatable(
        animation = tween(1000, easing = LinearEasing)
    )
)

CircularProgressIndicator(
    modifier = Modifier.rotate(angle),
    color = Color(0xFF7C3AED)
)
```

**Pulse Animation (Loading State):**
```kotlin
val alpha by infiniteTransition.animateFloat(
    initialValue = 0.3f,
    targetValue = 1f,
    animationSpec = infiniteRepeatable(
        animation = tween(1000, easing = FastOutSlowInEasing),
        repeatMode = RepeatMode.Reverse
    )
)

Box(
    modifier = Modifier
        .size(120.dp)
        .alpha(alpha)
        .background(Color(0xFF7C3AED), shape = CircleShape)
)
```

### Success Animations

**Checkmark Appear:**
```kotlin
// Circular reveal + scale
val scale by animateFloatAsState(
    targetValue = if (success) 1f else 0f,
    animationSpec = spring(
        dampingRatio = Spring.DampingRatioMediumBouncy,
        stiffness = Spring.StiffnessLow
    )
)

Icon(
    imageVector = Icons.Default.CheckCircle,
    contentDescription = "Success",
    tint = Color(0xFF10B981),
    modifier = Modifier
        .size(80.dp)
        .scale(scale)
)
```

### List Animations

**Staggered Fade-In:**
```kotlin
LazyColumn {
    itemsIndexed(items) { index, item ->
        val visible by remember {
            mutableStateOf(false)
        }
        LaunchedEffect(Unit) {
            delay(index * 50L) // Stagger by 50ms per item
            visible = true
        }

        AnimatedVisibility(
            visible = visible,
            enter = fadeIn(animationSpec = tween(300)) +
                    slideInVertically(initialOffsetY = { it / 2 })
        ) {
            ListItem(item)
        }
    }
}
```

### Haptic Feedback

**Trigger Points:**
- Button press: Light impact
- Toggle switch: Medium impact
- Error state: Notification feedback
- Success state: Success feedback
- Selection: Selection feedback

```kotlin
val hapticFeedback = LocalHapticFeedback.current

Button(
    onClick = {
        hapticFeedback.performHapticFeedback(HapticFeedbackType.LongPress)
        // Action
    }
) {
    Text("확인")
}
```

---

## 7. Screen-by-Screen Analysis

### 7.1 Splash Screen

**Layout:**
- Full screen purple gradient background (`#8B5CF6` → `#7C3AED`)
- Centered logo with animated pills (colorful dots orbiting "P")
- Progress bar at bottom (white, thin)

**Key Elements:**
- Logo size: `~120.dp` width
- Vertical centering with slight upward bias
- Progress bar: `4.dp` height, `16.dp` horizontal margin

**For Lottomate:**
```kotlin
Box(
    modifier = Modifier
        .fillMaxSize()
        .background(
            brush = Brush.verticalGradient(
                colors = listOf(Color(0xFF8B5CF6), Color(0xFF7C3AED))
            )
        ),
    contentAlignment = Alignment.Center
) {
    // Lottomate logo with animated lottery balls
    Image(
        painter = painterResource(R.drawable.logo_lottomate),
        contentDescription = "Lottomate",
        modifier = Modifier.size(120.dp)
    )

    // Bottom progress bar
    LinearProgressIndicator(
        modifier = Modifier
            .fillMaxWidth()
            .padding(horizontal = 16.dp)
            .align(Alignment.BottomCenter)
            .padding(bottom = 48.dp),
        color = Color.White,
        trackColor = Color.White.copy(alpha = 0.3f)
    )
}
```

### 7.2 Home Screen

**Layout Structure:**
- Purple top app bar with logo, notifications, profile icons
- White search bar with rounded corners (24.dp radius)
- Content sections with cards
- Bottom navigation (5 items)

**Section Pattern:**
```kotlin
LazyColumn {
    item {
        // Hero section with greeting
        Card(backgroundColor = Color(0xFFF3E8FF)) {
            Column {
                Text("일지말고 챙겨드세요 ⏰", style = headlineSmall)
                // Intake reminders
            }
        }
    }

    item {
        Spacer(Modifier.height(32.dp)) // Section spacing
    }

    item {
        // Recommendations section
        Text("필라이즈 추천", style = headlineMedium)
        Spacer(Modifier.height(16.dp))
        HorizontalProductGrid()
    }

    item {
        Spacer(Modifier.height(32.dp))
    }

    item {
        // Q&A section
        Text("Q&A", style = headlineMedium)
        Text("지금 필라이즈 질문", subtitle)
        Spacer(Modifier.height(16.dp))
        QnAList()
    }
}
```

**Key Patterns:**
- Section titles: `headlineMedium`, bold
- Section spacing: `32.dp`
- Title-to-content spacing: `16.dp`
- Cards have `12.dp` vertical margin

**For Lottomate Home:**
- Hero card: Recent winning numbers or user's saved numbers
- Quick actions: QR scan, manual input, statistics
- Recent draws section
- My numbers section

### 7.3 Search/Analysis Screen

**Layout:**
- Back button + search bar in top section
- Search bar: Light gray background, no border, rounded `24.dp`
- Quick action buttons with emoji icons
- Recent searches as chips
- Results as cards or list items

**Search Bar:**
```kotlin
OutlinedTextField(
    value = query,
    onValueChange = { query = it },
    modifier = Modifier.fillMaxWidth(),
    placeholder = { Text("제품명, 브랜드명, 증상으로 검색") },
    leadingIcon = { Icon(Icons.Default.Search) },
    trailingIcon = {
        if (query.isNotEmpty()) {
            IconButton(onClick = { query = "" }) {
                Icon(Icons.Default.Close)
            }
        }
    },
    shape = RoundedCornerShape(24.dp),
    colors = OutlinedTextFieldDefaults.colors(
        focusedBorderColor = Color.Transparent,
        unfocusedBorderColor = Color.Transparent,
        containerColor = Color(0xFFF3F4F6)
    )
)
```

**Quick Actions:**
```kotlin
Row(
    modifier = Modifier.fillMaxWidth(),
    horizontalArrangement = Arrangement.spacedBy(12.dp)
) {
    QuickActionCard(
        emoji = "📝",
        title = "성분으로 검색하기",
        modifier = Modifier.weight(1f)
    )
    QuickActionCard(
        emoji = "🫱",
        title = "건강고민으로 검색하기",
        modifier = Modifier.weight(1f)
    )
}
```

**Empty State:**
```kotlin
if (results.isEmpty() && query.isNotEmpty()) {
    EmptyState(
        emoji = "👻",
        title = "아직 선택하신 영양제가 없어요"
    )
}
```

**For Lottomate:**
- Search for past draw numbers
- Quick actions: Scan QR, Enter numbers, View statistics
- Recent searches: Draw numbers, dates

### 7.4 Results/Detail Screen

**Layout:**
- Top section: Product image, name, rating
- Tabbed content: 상세 정보 / 리뷰
- Sticky tabs below image
- Scrollable content areas

**Result Card (Analysis Complete):**
```kotlin
Card(
    modifier = Modifier.fillMaxWidth(),
    shape = RoundedCornerShape(20.dp),
    colors = CardDefaults.cardColors(
        containerColor = Color(0xFFF3E8FF) // Light purple
    )
) {
    Column(
        modifier = Modifier.padding(20.dp),
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        // Summary grid
        Row {
            StatusBadge("부족", "6개", BadgeType.EXCESS)
            Spacer(Modifier.width(8.dp))
            StatusBadge("주의", "4개", BadgeType.WARNING)
        }
        Spacer(Modifier.height(8.dp))
        Row {
            StatusBadge("최적", "2개", BadgeType.OPTIMAL)
            Spacer(Modifier.width(8.dp))
            StatusBadge("최소", "12개", BadgeType.MINIMAL)
        }

        Spacer(Modifier.height(16.dp))

        Text(
            text = "정민님의 건강데이터를 고려해 맞춤 분석했어요",
            style = MaterialTheme.typography.bodySmall,
            color = Color(0xFF6B7280)
        )
    }
}
```

**For Lottomate:**
- Winning numbers display with colored balls
- Prize breakdown (1등, 2등, etc.)
- User's numbers comparison
- Match indicators (colored highlights)

### 7.5 Onboarding/Signup Flow

**Layout:**
- Step indicator at top (progress dots or bars)
- Large title question
- Content area (form, selections, or illustrations)
- Bottom fixed button ("다음" or "확인")
- Progress indicator: Top of screen, `1/10` format

**Question Screen:**
```kotlin
Scaffold(
    topBar = {
        TopAppBar(
            navigationIcon = { BackButton() },
            actions = {
                Text("1/10", style = labelLarge, color = Color(0xFF9CA3AF))
            }
        )
    },
    bottomBar = {
        Button(
            onClick = { },
            modifier = Modifier
                .fillMaxWidth()
                .padding(16.dp),
            enabled = selectionValid
        ) {
            Text("다음")
        }
    }
) { padding ->
    Column(
        modifier = Modifier
            .padding(padding)
            .padding(horizontal = 16.dp)
            .fillMaxSize()
    ) {
        Text(
            text = "가입을 축하드려요!\n어떻게 불러드리면 될까요?",
            style = MaterialTheme.typography.displayMedium,
            fontWeight = FontWeight.Bold
        )

        Spacer(Modifier.height(24.dp))

        OutlinedTextField(
            value = name,
            onValueChange = { name = it },
            placeholder = { Text("5글자 내로 입력해주세요") },
            modifier = Modifier.fillMaxWidth()
        )

        Text(
            text = "0/5",
            modifier = Modifier.align(Alignment.End).padding(top = 4.dp),
            style = MaterialTheme.typography.labelSmall,
            color = Color(0xFF9CA3AF)
        )
    }
}
```

**Selection Grid:**
```kotlin
LazyVerticalGrid(
    columns = GridCells.Fixed(3),
    contentPadding = PaddingValues(16.dp),
    horizontalArrangement = Arrangement.spacedBy(12.dp),
    verticalArrangement = Arrangement.spacedBy(12.dp)
) {
    items(options) { option ->
        SelectionCard(
            illustration = option.emoji,
            label = option.name,
            selected = option.isSelected,
            badge = option.badge, // "20대 추천"
            onClick = { toggleSelection(option) }
        )
    }
}
```

**For Lottomate:**
- User name input
- Favorite number selection
- Notification preferences
- Tutorial screens with illustrations

### 7.6 Settings/Profile Screen

**Layout:**
- Top section: User info card
- List sections with dividers
- Each section has title + list items

**Profile Card:**
```kotlin
Card(
    modifier = Modifier.fillMaxWidth(),
    shape = RoundedCornerShape(16.dp)
) {
    Row(
        modifier = Modifier.padding(16.dp),
        verticalAlignment = Alignment.CenterVertically
    ) {
        // Profile emoji
        Text("👋", fontSize = 48.sp)

        Spacer(Modifier.width(16.dp))

        Column {
            Text(
                text = "WWIT님",
                style = MaterialTheme.typography.headlineSmall,
                fontWeight = FontWeight.Bold
            )
            Text(
                text = "오늘 건강하세요!",
                style = MaterialTheme.typography.bodyMedium,
                color = Color(0xFF6B7280)
            )
        }

        Spacer(Modifier.weight(1f))

        // Emoji sentiment indicator
        Text("😊", fontSize = 32.sp)
    }
}
```

**Settings List:**
```kotlin
Column {
    SectionHeader("내 영양제 & 루틴")
    SettingsItem("영양제 관리", onClick = { })
    SettingsItem("나의 Q&A 보러가기", trailingBadge = "2", onClick = { })

    Divider(Modifier.padding(vertical = 16.dp))

    SectionHeader("건강 기록")
    SettingsItem("기본설정 1", onClick = { })
    SettingsItem("검사결과 1", onClick = { })

    Divider(Modifier.padding(vertical = 16.dp))

    SectionHeader("고객 지원")
    SettingsItem("앱 공유하기", onClick = { })
    SettingsItem("버그 제보 및 문의", onClick = { })
}

@Composable
fun SettingsItem(
    title: String,
    subtitle: String? = null,
    trailingBadge: String? = null,
    onClick: () -> Unit
) {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .clickable(onClick = onClick)
            .padding(horizontal = 16.dp, vertical = 12.dp),
        verticalAlignment = Alignment.CenterVertically
    ) {
        Column(modifier = Modifier.weight(1f)) {
            Text(title, style = MaterialTheme.typography.bodyLarge)
            subtitle?.let {
                Text(it, style = MaterialTheme.typography.bodySmall, color = Color(0xFF6B7280))
            }
        }

        trailingBadge?.let {
            Badge(
                containerColor = Color(0xFFEF4444),
                contentColor = Color.White
            ) {
                Text(it, style = MaterialTheme.typography.labelSmall)
            }
            Spacer(Modifier.width(8.dp))
        }

        Icon(
            imageVector = Icons.Default.ChevronRight,
            contentDescription = null,
            tint = Color(0xFF9CA3AF)
        )
    }
}
```

**For Lottomate:**
- User statistics card (total numbers saved, wins)
- Number management
- Notification settings
- App preferences

---

## 8. Data Visualization Patterns

### Chart Styles

#### Line Chart (Health Condition)
```kotlin
Canvas(modifier = Modifier.fillMaxWidth().height(200.dp)) {
    val points = dataPoints.mapIndexed { index, value ->
        Offset(
            x = size.width * index / (dataPoints.size - 1),
            y = size.height * (1 - value)
        )
    }

    // Draw path
    val path = Path().apply {
        moveTo(points.first().x, points.first().y)
        points.forEach { point ->
            lineTo(point.x, point.y)
        }
    }

    drawPath(
        path = path,
        color = Color(0xFF7C3AED),
        style = Stroke(width = 4.dp.toPx(), cap = StrokeCap.Round)
    )

    // Draw emoji indicators at data points
    points.forEach { point ->
        drawCircle(
            color = Color(0xFF7C3AED),
            radius = 6.dp.toPx(),
            center = point
        )
    }
}
```

**Properties:**
- Line: `4.dp` width, purple, rounded caps
- Data points: `12.dp` diameter circles
- Y-axis: Emoji sentiment indicators
- Grid: Subtle gray lines `#E5E7EB`
- No axis labels (emoji-based)

#### Bar Chart (Statistics)
```kotlin
Row(
    modifier = Modifier.fillMaxWidth(),
    horizontalArrangement = Arrangement.spacedBy(8.dp)
) {
    dataPoints.forEach { (label, value, percentage) ->
        Column(
            modifier = Modifier.weight(1f),
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            // Bar
            Box(
                modifier = Modifier
                    .width(40.dp)
                    .height((200 * percentage).dp)
                    .background(
                        color = Color(0xFF7C3AED),
                        shape = RoundedCornerShape(topStart = 8.dp, topEnd = 8.dp)
                    )
            )

            Spacer(Modifier.height(8.dp))

            // Label
            Text(
                text = label,
                style = MaterialTheme.typography.labelSmall,
                color = Color(0xFF6B7280)
            )

            // Value
            Text(
                text = value,
                style = MaterialTheme.typography.labelMedium,
                fontWeight = FontWeight.Bold
            )
        }
    }
}
```

**Properties:**
- Bar width: `40.dp`
- Bar color: Purple `#7C3AED`
- Corner radius: `8.dp` (top only)
- Spacing: `8.dp` between bars
- Labels below bars

#### Pie/Donut Chart (Sentiment Distribution)
```kotlin
// Emoji-based sentiment distribution
Row(
    modifier = Modifier.fillMaxWidth(),
    horizontalArrangement = Arrangement.SpaceBetween
) {
    sentimentData.forEach { (emoji, percentage) ->
        Column(
            horizontalAlignment = Alignment.CenterHorizontally,
            modifier = Modifier.weight(1f)
        ) {
            // Emoji
            Text(emoji, fontSize = 40.sp)

            Spacer(Modifier.height(8.dp))

            // Percentage
            Text(
                text = "$percentage%",
                style = MaterialTheme.typography.bodyLarge,
                fontWeight = FontWeight.Bold
            )
        }
    }
}
```

**For Lottomate:**
- Number frequency bars (horizontal or vertical)
- Winning probability pie chart
- Draw history line chart
- Number distribution heatmap

---

## 9. Implementation Guide for Lottomate

### Phase 1: Foundation (Week 1-2)

#### 1.1 Design System Setup

**Create `core:design-system` module structure:**
```
core/design-system/src/main/kotlin/com/yunseong/core/designsystem/
├── theme/
│   ├── Color.kt           # Color palette
│   ├── Typography.kt      # Type scale
│   ├── Shape.kt          # Corner radii
│   └── Theme.kt          # Material3 theme
├── component/
│   ├── Button.kt         # LottoButton variants
│   ├── Card.kt           # LottoCard
│   ├── TextField.kt      # LottoTextField
│   ├── Chip.kt           # LottoChip
│   └── EmptyState.kt     # EmptyState
└── foundation/
    ├── Spacing.kt        # Spacing constants
    └── Icons.kt          # Custom icons
```

**Color.kt:**
```kotlin
package com.yunseong.core.designsystem.theme

import androidx.compose.ui.graphics.Color

// Primary Colors
val LottoPurple = Color(0xFF7C3AED)
val LottoPurpleDark = Color(0xFF6D28D9)
val LottoPurpleLight = Color(0xFFE9D5FF)

// Status Colors
val LottoWarning = Color(0xFFFCD34D)
val LottoDanger = Color(0xFFFDA4AF)
val LottoSuccess = Color(0xFF67E8F9)
val LottoInfo = Color(0xFF6EE7B7)

// Neutrals
val LottoGray50 = Color(0xFFF9FAFB)
val LottoGray100 = Color(0xFFF3F4F6)
val LottoGray200 = Color(0xFFE5E7EB)
val LottoGray300 = Color(0xFFD1D5DB)
val LottoGray400 = Color(0xFF9CA3AF)
val LottoGray500 = Color(0xFF6B7280)
val LottoGray600 = Color(0xFF4B5563)
val LottoGray700 = Color(0xFF374151)
val LottoGray800 = Color(0xFF1F2937)
val LottoGray900 = Color(0xFF111827)

// Semantic aliases
val TextPrimary = LottoGray900
val TextSecondary = LottoGray500
val TextTertiary = LottoGray400
val BackgroundPrimary = Color.White
val BackgroundSecondary = LottoGray50
val Surface = Color.White
val SurfaceVariant = LottoGray100
val Border = LottoGray200
```

**Typography.kt:**
```kotlin
package com.yunseong.core.designsystem.theme

import androidx.compose.material3.Typography
import androidx.compose.ui.text.TextStyle
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.sp

val LottoTypography = Typography(
    displayLarge = TextStyle(
        fontSize = 28.sp,
        fontWeight = FontWeight.Bold,
        lineHeight = 36.sp,
        letterSpacing = (-0.5).sp
    ),
    displayMedium = TextStyle(
        fontSize = 24.sp,
        fontWeight = FontWeight.Bold,
        lineHeight = 32.sp,
        letterSpacing = (-0.3).sp
    ),
    headlineSmall = TextStyle(
        fontSize = 20.sp,
        fontWeight = FontWeight.Bold,
        lineHeight = 28.sp
    ),
    titleLarge = TextStyle(
        fontSize = 18.sp,
        fontWeight = FontWeight.SemiBold,
        lineHeight = 24.sp
    ),
    titleMedium = TextStyle(
        fontSize = 16.sp,
        fontWeight = FontWeight.SemiBold,
        lineHeight = 22.sp
    ),
    bodyLarge = TextStyle(
        fontSize = 16.sp,
        fontWeight = FontWeight.Normal,
        lineHeight = 24.sp
    ),
    bodyMedium = TextStyle(
        fontSize = 14.sp,
        fontWeight = FontWeight.Normal,
        lineHeight = 20.sp
    ),
    bodySmall = TextStyle(
        fontSize = 13.sp,
        fontWeight = FontWeight.Normal,
        lineHeight = 18.sp
    ),
    labelLarge = TextStyle(
        fontSize = 16.sp,
        fontWeight = FontWeight.SemiBold,
        lineHeight = 20.sp
    ),
    labelMedium = TextStyle(
        fontSize = 14.sp,
        fontWeight = FontWeight.Medium,
        lineHeight = 18.sp
    ),
    labelSmall = TextStyle(
        fontSize = 12.sp,
        fontWeight = FontWeight.Medium,
        lineHeight = 16.sp
    )
)
```

**Spacing.kt:**
```kotlin
package com.yunseong.core.designsystem.foundation

import androidx.compose.ui.unit.dp

object Spacing {
    val xxxs = 2.dp
    val xxs = 4.dp
    val xs = 8.dp
    val sm = 12.dp
    val md = 16.dp
    val lg = 20.dp
    val xl = 24.dp
    val xxl = 32.dp
    val xxxl = 40.dp
    val huge = 48.dp
}
```

#### 1.2 Core Components

**LottoButton.kt:**
```kotlin
package com.yunseong.core.designsystem.component

import androidx.compose.foundation.layout.*
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import com.yunseong.core.designsystem.theme.LottoPurple

@Composable
fun LottoButton(
    text: String,
    onClick: () -> Unit,
    modifier: Modifier = Modifier,
    enabled: Boolean = true,
    variant: LottoButtonVariant = LottoButtonVariant.Primary
) {
    when (variant) {
        LottoButtonVariant.Primary -> {
            Button(
                onClick = onClick,
                modifier = modifier
                    .fillMaxWidth()
                    .height(56.dp),
                enabled = enabled,
                shape = RoundedCornerShape(16.dp),
                colors = ButtonDefaults.buttonColors(
                    containerColor = LottoPurple,
                    contentColor = Color.White
                )
            ) {
                Text(
                    text = text,
                    style = MaterialTheme.typography.labelLarge,
                    fontWeight = FontWeight.SemiBold
                )
            }
        }
        LottoButtonVariant.Secondary -> {
            OutlinedButton(
                onClick = onClick,
                modifier = modifier
                    .fillMaxWidth()
                    .height(48.dp),
                enabled = enabled,
                shape = RoundedCornerShape(16.dp),
                border = ButtonDefaults.outlinedButtonBorder,
                colors = ButtonDefaults.outlinedButtonColors(
                    contentColor = LottoPurple
                )
            ) {
                Text(
                    text = text,
                    style = MaterialTheme.typography.labelMedium
                )
            }
        }
        LottoButtonVariant.Text -> {
            TextButton(
                onClick = onClick,
                modifier = modifier,
                enabled = enabled
            ) {
                Text(
                    text = text,
                    style = MaterialTheme.typography.labelMedium
                )
            }
        }
    }
}

enum class LottoButtonVariant {
    Primary, Secondary, Text
}
```

**LottoCard.kt:**
```kotlin
package com.yunseong.core.designsystem.component

import androidx.compose.foundation.layout.*
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.unit.dp

@Composable
fun LottoCard(
    modifier: Modifier = Modifier,
    backgroundColor: Color = Color.White,
    onClick: (() -> Unit)? = null,
    content: @Composable ColumnScope.() -> Unit
) {
    Card(
        modifier = modifier.fillMaxWidth(),
        onClick = onClick ?: {},
        shape = RoundedCornerShape(16.dp),
        colors = CardDefaults.cardColors(
            containerColor = backgroundColor
        ),
        elevation = CardDefaults.cardElevation(defaultElevation = 0.dp)
    ) {
        Column(
            modifier = Modifier.padding(16.dp),
            verticalArrangement = Arrangement.spacedBy(12.dp),
            content = content
        )
    }
}
```

### Phase 2: Screen Implementation (Week 3-4)

#### 2.1 Update Home Screen

**Apply Pillyze patterns to existing home screen:**

```kotlin
@Composable
fun LottomateHomeScreen(
    viewModel: HomeViewModel = hiltViewModel()
) {
    val uiState by viewModel.uiState.collectAsState()

    Scaffold(
        topBar = {
            LottomateTopBar(
                title = "Lottomate",
                actions = {
                    IconButton(onClick = { /* Notifications */ }) {
                        Icon(Icons.Default.Notifications, contentDescription = "알림")
                    }
                    IconButton(onClick = { /* Profile */ }) {
                        Icon(Icons.Default.Person, contentDescription = "프로필")
                    }
                }
            )
        },
        bottomBar = {
            LottomateBottomNavigation()
        }
    ) { padding ->
        LazyColumn(
            modifier = Modifier
                .padding(padding)
                .fillMaxSize(),
            contentPadding = PaddingValues(vertical = 16.dp),
            verticalArrangement = Arrangement.spacedBy(16.dp)
        ) {
            item {
                // Hero card - Recent draw
                LottoCard(
                    modifier = Modifier.padding(horizontal = 16.dp),
                    backgroundColor = LottoPurpleLight
                ) {
                    Text(
                        text = "🎰 ${uiState.latestDrawNumber}회차 당첨번호",
                        style = MaterialTheme.typography.headlineSmall
                    )
                    Spacer(Modifier.height(8.dp))
                    WinningNumberRow(numbers = uiState.latestWinningNumbers)
                }
            }

            item {
                // Quick actions
                QuickActionsSection(
                    onScanQR = { },
                    onManualInput = { },
                    onViewStatistics = { }
                )
            }

            item {
                Spacer(Modifier.height(16.dp))
            }

            item {
                // My numbers section
                SectionHeader(
                    title = "나의 번호",
                    action = "전체 보기",
                    onActionClick = { }
                )
            }

            items(uiState.myNumbers.take(3)) { numberSet ->
                MyNumberCard(
                    numberSet = numberSet,
                    onClick = { }
                )
            }
        }
    }
}

@Composable
fun WinningNumberRow(numbers: List<Int>) {
    Row(
        horizontalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        numbers.forEach { number ->
            LottoNumberBall(
                number = number,
                size = 40.dp
            )
        }
    }
}

@Composable
fun LottoNumberBall(
    number: Int,
    size: Dp = 40.dp
) {
    val ballColor = when {
        number <= 10 -> Color(0xFFFCD34D) // Yellow
        number <= 20 -> Color(0xFF60A5FA) // Blue
        number <= 30 -> Color(0xFFF87171) // Red
        number <= 40 -> Color(0xFF9CA3AF) // Gray
        else -> Color(0xFF34D399) // Green
    }

    Surface(
        modifier = Modifier.size(size),
        shape = CircleShape,
        color = ballColor
    ) {
        Box(contentAlignment = Alignment.Center) {
            Text(
                text = number.toString(),
                style = MaterialTheme.typography.labelLarge,
                color = Color.White,
                fontWeight = FontWeight.Bold
            )
        }
    }
}
```

#### 2.2 Create Number Input Screen

**Follow Pillyze's input flow pattern:**

```kotlin
@Composable
fun NumberInputScreen(
    onComplete: (List<Int>) -> Unit
) {
    var selectedNumbers by remember { mutableStateOf(setOf<Int>()) }

    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("번호 선택") },
                navigationIcon = {
                    IconButton(onClick = { /* Back */ }) {
                        Icon(Icons.Default.ArrowBack, contentDescription = "뒤로")
                    }
                }
            )
        },
        bottomBar = {
            LottoButton(
                text = "확인 (${selectedNumbers.size}/6)",
                onClick = { onComplete(selectedNumbers.toList()) },
                enabled = selectedNumbers.size == 6,
                modifier = Modifier.padding(16.dp)
            )
        }
    ) { padding ->
        Column(
            modifier = Modifier
                .padding(padding)
                .fillMaxSize()
        ) {
            Text(
                text = "번호를 선택해주세요",
                style = MaterialTheme.typography.displayMedium,
                fontWeight = FontWeight.Bold,
                modifier = Modifier.padding(horizontal = 16.dp, vertical = 16.dp)
            )

            Text(
                text = "6개의 번호를 선택하세요 (1~45)",
                style = MaterialTheme.typography.bodyMedium,
                color = TextSecondary,
                modifier = Modifier.padding(horizontal = 16.dp)
            )

            Spacer(Modifier.height(24.dp))

            // Number grid
            LazyVerticalGrid(
                columns = GridCells.Fixed(7),
                contentPadding = PaddingValues(16.dp),
                horizontalArrangement = Arrangement.spacedBy(8.dp),
                verticalArrangement = Arrangement.spacedBy(8.dp)
            ) {
                items(45) { index ->
                    val number = index + 1
                    val isSelected = selectedNumbers.contains(number)

                    LottoNumberBall(
                        number = number,
                        size = 48.dp,
                        selected = isSelected,
                        onClick = {
                            selectedNumbers = if (isSelected) {
                                selectedNumbers - number
                            } else if (selectedNumbers.size < 6) {
                                selectedNumbers + number
                            } else {
                                selectedNumbers
                            }
                        }
                    )
                }
            }
        }
    }
}
```

### Phase 3: Advanced Features (Week 5-6)

#### 3.1 Statistics Screen with Charts

```kotlin
@Composable
fun StatisticsScreen() {
    LazyColumn(
        contentPadding = PaddingValues(16.dp),
        verticalArrangement = Arrangement.spacedBy(24.dp)
    ) {
        item {
            Text(
                text = "번호 통계",
                style = MaterialTheme.typography.displayMedium,
                fontWeight = FontWeight.Bold
            )
        }

        item {
            LottoCard {
                Text(
                    text = "가장 많이 나온 번호",
                    style = MaterialTheme.typography.titleLarge
                )
                Spacer(Modifier.height(16.dp))
                FrequencyBarChart(data = frequencyData)
            }
        }

        item {
            LottoCard {
                Text(
                    text = "최근 10회차 추이",
                    style = MaterialTheme.typography.titleLarge
                )
                Spacer(Modifier.height(16.dp))
                DrawHistoryLineChart(data = historyData)
            }
        }
    }
}
```

#### 3.2 Empty States & Loading

```kotlin
@Composable
fun MyNumbersEmptyState() {
    EmptyState(
        emoji = "🎲",
        title = "저장된 번호가 없어요",
        description = "번호를 등록하고 당첨 여부를 확인하세요",
        actionButton = {
            LottoButton(
                text = "번호 추가하기",
                onClick = { },
                variant = LottoButtonVariant.Primary
            )
        }
    )
}

@Composable
fun CheckingLoadingState() {
    Box(
        modifier = Modifier.fillMaxSize(),
        contentAlignment = Alignment.Center
    ) {
        Column(
            horizontalAlignment = Alignment.CenterHorizontally,
            verticalArrangement = Arrangement.spacedBy(24.dp)
        ) {
            // Animated lottery ball illustration
            AnimatedLotteryBalls()

            CircularProgressIndicator(
                modifier = Modifier.size(40.dp),
                color = LottoPurple,
                strokeWidth = 4.dp
            )

            Text(
                text = "당첨 번호를 확인하고 있어요",
                style = MaterialTheme.typography.bodyLarge,
                fontWeight = FontWeight.Medium
            )
        }
    }
}
```

### Phase 4: Polish & Details (Week 7-8)

#### 4.1 Animations

```kotlin
// Success animation after checking numbers
@Composable
fun WinningSuccessAnimation(
    onDismiss: () -> Unit
) {
    var visible by remember { mutableStateOf(false) }

    LaunchedEffect(Unit) {
        visible = true
        delay(2000)
        onDismiss()
    }

    AnimatedVisibility(
        visible = visible,
        enter = fadeIn() + scaleIn(),
        exit = fadeOut() + scaleOut()
    ) {
        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(Color.Black.copy(alpha = 0.7f)),
            contentAlignment = Alignment.Center
        ) {
            Column(
                horizontalAlignment = Alignment.CenterHorizontally,
                verticalArrangement = Arrangement.spacedBy(16.dp)
            ) {
                // Checkmark animation
                Icon(
                    imageVector = Icons.Default.CheckCircle,
                    contentDescription = null,
                    tint = LottoSuccess,
                    modifier = Modifier.size(80.dp)
                )

                Text(
                    text = "축하합니다! 🎉",
                    style = MaterialTheme.typography.displayMedium,
                    color = Color.White,
                    fontWeight = FontWeight.Bold
                )

                Text(
                    text = "3등에 당첨되었어요",
                    style = MaterialTheme.typography.bodyLarge,
                    color = Color.White
                )
            }
        }
    }
}
```

#### 4.2 Haptic Feedback

```kotlin
@Composable
fun LottoNumberBallWithHaptic(
    number: Int,
    selected: Boolean,
    onClick: () -> Unit
) {
    val haptic = LocalHapticFeedback.current

    Surface(
        onClick = {
            haptic.performHapticFeedback(HapticFeedbackType.LongPress)
            onClick()
        },
        modifier = Modifier.size(48.dp),
        shape = CircleShape,
        color = if (selected) LottoPurple else LottoGray100
    ) {
        Box(contentAlignment = Alignment.Center) {
            Text(
                text = number.toString(),
                color = if (selected) Color.White else TextPrimary,
                fontWeight = FontWeight.Bold
            )
        }
    }
}
```

### Implementation Priority Matrix

| Priority | Component | Effort | Impact | Status |
|----------|-----------|--------|--------|--------|
| HIGH | Color system | Low | High | Phase 1 |
| HIGH | Typography | Low | High | Phase 1 |
| HIGH | Button components | Medium | High | Phase 1 |
| HIGH | Card components | Medium | High | Phase 1 |
| HIGH | Home screen redesign | High | High | Phase 2 |
| MEDIUM | Input fields | Medium | Medium | Phase 2 |
| MEDIUM | Number input screen | High | High | Phase 2 |
| MEDIUM | Empty states | Low | Medium | Phase 3 |
| MEDIUM | Loading states | Medium | Medium | Phase 3 |
| MEDIUM | Charts/visualization | High | Medium | Phase 3 |
| LOW | Animations | Medium | Low | Phase 4 |
| LOW | Haptic feedback | Low | Low | Phase 4 |
| LOW | 3D illustrations | High | Low | Phase 4 |

---

## 10. Key Takeaways & Recommendations

### What Makes Pillyze's Design Work

1. **Consistent Color Usage**: Purple brand color appears consistently across all interactive elements
2. **Generous Spacing**: 16.dp horizontal padding and 32.dp section spacing creates breathing room
3. **Rounded Everything**: 16.dp corner radius for cards, 24.dp for search bars creates friendly feel
4. **Emoji-Driven Communication**: Reduces cognitive load, adds personality
5. **Flat Design**: No shadows or gradients on cards (except illustrations)
6. **Clear Hierarchy**: Bold headings, structured content, visual separation
7. **Status Color System**: Semantic colors (부족/주의/최적/최소) provide instant feedback

### Recommendations for Lottomate

#### Must Implement (High Priority)
1. **Adopt Purple Primary Color** (`#7C3AED`) for brand consistency
2. **16.dp Corner Radius** for all cards and containers
3. **Emoji Usage** for number status (🎯 match, ❌ no match, 🏆 jackpot)
4. **Status Badge System** for prize tiers (1등=success color, 2등=info, etc.)
5. **Empty States** with friendly ghost emoji and encouraging copy
6. **Loading States** with animated lottery balls

#### Should Implement (Medium Priority)
1. **Bottom Navigation** with 5 icons (home, check numbers, statistics, my numbers, profile)
2. **Search Bar** with rounded corners for past draw search
3. **Card-Based Layout** for all content sections
4. **Number Ball Component** with color coding (1-10 yellow, 11-20 blue, etc.)
5. **Success Animations** for winning checks

#### Nice to Have (Low Priority)
1. **3D Lottery Ball Illustrations** for splash/hero sections
2. **Chart Visualizations** for number frequency
3. **Haptic Feedback** on number selection
4. **Gradient Backgrounds** for special sections
5. **Staggered Animations** for list items

### Design System Checklist

- [ ] Color palette defined in `Color.kt`
- [ ] Typography scale defined in `Typography.kt`
- [ ] Spacing constants defined in `Spacing.kt`
- [ ] Button variants created (`LottoButton`)
- [ ] Card component created (`LottoCard`)
- [ ] Text field component created (`LottoTextField`)
- [ ] Chip/Badge components created
- [ ] Empty state component created
- [ ] Loading indicator component created
- [ ] Bottom navigation implemented
- [ ] Top app bar variants created
- [ ] Status badge system implemented
- [ ] Number ball component created
- [ ] Animation utilities created

### Final Notes

Pillyze's design system is **highly transferable** to Lottomate because:
1. Both are consumer-facing apps requiring trust + approachability
2. Both handle numerical data (vitamins vs lottery numbers)
3. Both need clear status communication (health vs winning status)
4. Both benefit from gamification elements (emoji, animations)

The key is to **adapt, not copy**. Use Pillyze's foundational patterns (colors, spacing, components) but customize illustrations and content for lottery context. The purple brand color works well for both health and lottery (associated with premium, trust, and excitement).

**Estimated Implementation Time**: 8 weeks with 1 developer
- Week 1-2: Design system foundation
- Week 3-4: Core screens (home, input, results)
- Week 5-6: Advanced features (statistics, charts)
- Week 7-8: Polish, animations, testing

---

**Document Version**: 1.0
**Last Updated**: 2024-12-23
**Total Analysis**: 129 screenshots from Pillyze app
**Target Application**: Lottomate (로또 당첨 확인 앱)
