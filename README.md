# Rohit's Multi-Page Blog - Canvas Protocol Demo Site

## 🎯 What This Is

A realistic multi-page blog that demonstrates Canvas Protocol integration:
- **Multiple pages** you can navigate between (Home, 3 articles, About)
- **CAPTCHA appears once** on first visit to any page
- **After verification** user can browse all pages freely
- **Feels like a real site** with proper navigation and content

## 📁 Files

```
/multipage-blog/
├── index.html              # Homepage with article grid
├── article-bitcoin.html    # Bitcoin article
├── article-ai.html         # AI article
├── article-defi.html       # DeFi article
├── about.html              # About page
└── styles.css              # Shared stylesheet
```

## 🔐 How Canvas SDK Should Work

### First Visit Flow:
```
User visits index.html (or any page)
    ↓
Deep's SDK checks localStorage for "canvas_verified" token
    ↓
No token found
    ↓
SDK injects full-page CAPTCHA overlay
    ↓
User completes CAPTCHA
    ↓
EigenCompute verifies
    ↓
SDK stores token: localStorage.setItem('canvas_verified', 'true')
    ↓
Overlay removes, user sees homepage
    ↓
User clicks to article-bitcoin.html
    ↓
SDK checks localStorage - token exists!
    ↓
No CAPTCHA shown, user browses freely
```

### Subsequent Visits:
```
User returns to site (any page)
    ↓
SDK checks localStorage
    ↓
Token exists from previous verification
    ↓
No CAPTCHA, instant access
```

## 💻 SDK Integration Code

Deep needs to add this to **every page** (or in a shared header include):

```html
<!-- Add before closing </body> tag on every page -->
<script src="https://sdk.canvas-protocol.xyz/canvas-sdk.js"></script>
<script>
  CanvasProtocol.init({
    publisherId: 'rohits-blog',
    mode: 'site-gate',
    checkVerification: true,  // Check if already verified
    onSuccess: function(result) {
      // Store verification token
      localStorage.setItem('canvas_verified', 'true');
      localStorage.setItem('canvas_verified_at', Date.now());
      console.log('User verified!');
    }
  });
</script>
```

## 🔧 SDK Logic Deep Needs to Build

```javascript
window.CanvasProtocol = {
  init: function(config) {
    // 1. Check if already verified
    const isVerified = localStorage.getItem('canvas_verified');
    
    if (isVerified) {
      console.log('User already verified, no CAPTCHA needed');
      return; // Exit - don't show overlay
    }
    
    // 2. Not verified - show CAPTCHA overlay
    console.log('First visit - showing CAPTCHA');
    this.showOverlay(config);
  },
  
  showOverlay: function(config) {
    // Inject full-page overlay
    // Load game iframe
    // Handle verification
    // On success: call config.onSuccess()
  }
};
```

## 📱 User Experience

**First Visit:**
1. User types in URL or clicks link
2. Page starts loading
3. **Immediately** full-page CAPTCHA appears (before content visible)
4. User completes Nike/CryptoVault game
5. Success message shows
6. Overlay fades out
7. User sees homepage content
8. Can now navigate entire site without seeing CAPTCHA again

**Return Visit:**
1. User visits any page
2. No CAPTCHA (token exists)
3. Instant access to content

## 🎨 Styling Notes

- Navigation is sticky (stays at top when scrolling)
- Responsive design works on mobile
- Article cards have hover effects
- Color scheme: Purple gradient (#667eea to #764ba2)
- All pages use shared `styles.css`

## 🚀 Hosting Instructions

### Option 1: GitHub Pages (Recommended)
1. Create repo `rohit-blog-demo`
2. Upload ALL files (keep folder structure)
3. Enable GitHub Pages
4. URL: `https://your-username.github.io/rohit-blog-demo/`

### Option 2: Netlify
1. Drag entire folder to app.netlify.com/drop
2. Get URL: `https://rohit-blog.netlify.app/`

**Important:** You must upload ALL files including `styles.css` or the site won't look right!

## ✅ Testing Checklist

After hosting:
- [ ] Visit homepage - navigation works
- [ ] Click "Bitcoin ATH" article - loads properly
- [ ] Click "AI Trends" article - loads properly
- [ ] Click "DeFi 2025" article - loads properly
- [ ] Click "About" page - loads properly
- [ ] All pages show header/footer
- [ ] Navigation highlights active page
- [ ] Mobile responsive (test on phone)

## 🔌 For Deep: Integration Points

**What you need to add:**

1. **SDK script tags on every page** (or include in a header template)
2. **localStorage check** before showing overlay
3. **Token storage** after successful verification
4. **Optional: Token expiration** - e.g. expire after 30 days

**Recommended SDK behavior:**

```javascript
// Check verification on page load
function checkVerification() {
  const verified = localStorage.getItem('canvas_verified');
  const timestamp = localStorage.getItem('canvas_verified_at');
  
  if (!verified) return false; // Never verified
  
  // Optional: Check if token expired (30 days)
  const daysSince = (Date.now() - parseInt(timestamp)) / (1000 * 60 * 60 * 24);
  if (daysSince > 30) {
    // Token expired, clear and re-verify
    localStorage.removeItem('canvas_verified');
    localStorage.removeItem('canvas_verified_at');
    return false;
  }
  
  return true; // Valid verification
}
```

## 🎬 Demo Day Presentation

**Show this flow:**

1. **Open homepage** - "Here's a normal multi-page blog"
2. **Show navigation** - "Multiple articles users can read"
3. **Explain:** "When user first visits, they see CAPTCHA"
4. **Show overlay demo** - "Full-page verification appears"
5. **Complete CAPTCHA** - "User plays Nike game"
6. **Success** - "Verification complete, stored in localStorage"
7. **Navigate pages** - "Now user can browse freely without seeing CAPTCHA again"
8. **Open different page** - "Token persists across pages"

## 📊 Why This Approach Works

**Benefits:**
- ✅ Realistic user experience (verify once, browse forever)
- ✅ No friction after first verification
- ✅ Works across all pages automatically
- ✅ Simple localStorage implementation
- ✅ Publisher gets paid once per user
- ✅ User doesn't have to verify on every page

**Alternative approaches (not as good):**
- ❌ CAPTCHA on every page load (too much friction)
- ❌ CAPTCHA only on specific pages (confusing UX)
- ❌ Time-based re-verification (annoying for users)

## 🚀 Ready to Deploy!

This is a production-ready demo site. Just:
1. Upload to hosting
2. Deep adds SDK integration
3. Test verification flow
4. Use for demo day!

---

Built for Canvas Protocol Buenos Aires demo (Nov 10-17, 2025)
