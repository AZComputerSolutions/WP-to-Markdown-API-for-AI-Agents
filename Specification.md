# WP-to-Markdown API for AI Agents
## Product Specification

---

**Document Information**

| Field | Value |
|-------|-------|
| **Project Name** | WP-to-Markdown API for AI Agents |
| **Document Type** | Product Specification |
| **Author** | Product Development Team |
| **Version** | 1.0 |
| **Status** | Draft |
| **Last Updated** | January 2025 |

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Objectives & User Personas](#2-objectives--user-personas)
3. [Functional Requirements](#3-functional-requirements)
4. [Non-Functional Requirements](#4-non-functional-requirements)
5. [Security Requirements](#5-security-requirements)
6. [Technical Stack & Implementation](#6-technical-stack--implementation)
7. [Page Builder Compatibility](#7-page-builder-compatibility)
8. [AI Markdown Discovery Protocol](#8-ai-markdown-discovery-protocol)
9. [Testing Requirements](#9-testing-requirements)
10. [Documentation Requirements](#10-documentation-requirements)
11. [Deployment & Distribution](#11-deployment--distribution)
12. [Success Metrics](#12-success-metrics)
13. [Acceptance Criteria](#13-acceptance-criteria)
14. [Future Roadmap](#14-future-roadmap)
15. [Appendix](#15-appendix)

---

# 1. Executive Summary

## 1.1 Overview

As AI agents, LLM-based crawlers, and custom GPTs increasingly browse the web for real-time data, traditional HTML pages present unnecessary clutter (scripts, styles, navigation menus). This WordPress plugin provides a lightweight, performant solution that automatically converts WordPress page/post content into clean Markdown format, exposed via a dedicated endpoint specifically optimized for AI consumption.

## 1.2 Problem Statement

Current challenges for AI agents consuming web content:

- **HTML Complexity:** Scripts, styles, and navigation elements add noise and consume tokens
- **Inconsistent Structure:** Every site has different DOM structures, making parsing unreliable
- **Token Waste:** HTML markup consumes valuable context window tokens in LLMs
- **No Discovery Standard:** AI bots cannot easily find machine-readable endpoints
- **Performance Impact:** Repeated parsing strains both client and server resources
- **Page Builder Complexity:** Modern WordPress sites use Elementor, Divi, WPBakery, and other builders that generate complex nested markup

## 1.3 Solution Overview

This plugin provides a comprehensive solution through:

1. **Markdown Endpoints:** Clean, token-efficient content delivery via multiple access methods
2. **Discovery Protocol:** Standardized methods for AI bots to find endpoints automatically
3. **Admin Controls:** Granular configuration for security, access, and performance
4. **Caching Layer:** High-performance response delivery with automatic invalidation
5. **Analytics:** Visibility into AI bot traffic, usage patterns, and performance
6. **Page Builder Support:** Intelligent content extraction from all major WordPress page builders

## 1.4 Key Benefits

| Benefit | Description |
|---------|-------------|
| **Token Efficiency** | Markdown reduces content size by 60-80% compared to raw HTML |
| **Structured Data** | Clean, predictable format with YAML metadata front-matter |
| **Performance** | Cached responses ensure minimal server impact (target: <100ms cached) |
| **Control** | Administrators maintain full control over AI-accessible content |
| **Discoverability** | Standardized protocol enables automatic bot discovery |
| **Compatibility** | Works with all major page builders and WordPress configurations |
| **Security** | Multiple authentication methods and rate limiting protect resources |

---

# 2. Objectives & User Personas

## 2.1 Core Objectives

| Objective | Description | Success Indicator |
|-----------|-------------|-------------------|
| **AI-Friendly Data Delivery** | Deliver website content in a highly structured, token-efficient Markdown format | Successful parsing by major AI agents |
| **Security & Access Control** | Allow site administrators to control which parts of the site AI bots can read | Zero unauthorized data exposure |
| **Performance Optimization** | Ensure fast response times to prevent bot traffic from draining server resources | <100ms cached response time |
| **Extensibility** | Provide hooks and filters for developers to customize behavior | Comprehensive hook documentation |
| **Universal Compatibility** | Work seamlessly with popular themes, plugins, and page builders | 95%+ compatibility rate |
| **Bot Discoverability** | Enable AI bots to automatically discover Markdown endpoints | Protocol adoption by major AI crawlers |
| **Page Builder Support** | Accurately extract content from Elementor, Divi, WPBakery, Beaver Builder, Oxygen, and Bricks | Clean Markdown from all major builders |

## 2.2 User Personas

### 2.2.1 The AI Agent/Robot

**Profile:** Automated system accessing the site via API or specific headers to ingest content for processing, training, or real-time responses.

**Examples:**
- GPTBot (OpenAI)
- ClaudeBot (Anthropic)
- PerplexityBot
- GoogleBot (for AI features)
- Custom enterprise AI crawlers
- Research institution crawlers

**Goals:**
- Quickly ingest page context without parsing complex DOM structures
- Access structured metadata alongside content
- Respect rate limits and authentication requirements
- Discover available content efficiently through standard protocols

**Pain Points:**
- HTML clutter requiring extensive parsing
- Inconsistent formatting across websites
- Rate limiting without clear guidelines
- Authentication barriers without documentation
- No standard discovery mechanism
- Page builder markup is extremely complex

**Needs:**
- Clean Markdown output with consistent structure
- Reliable, well-documented endpoints
- Clear error messages with actionable information
- Structured metadata in predictable format
- Discoverable endpoints via standard protocol
- Accurate content from page builders

### 2.2.2 The WordPress Administrator

**Profile:** Site owner or manager wanting to make content AI-accessible while maintaining security and performance.

**Examples:**
- Blog owners wanting AI visibility
- Content marketers optimizing for AI search engines
- Documentation site managers
- News publishers and media companies
- E-commerce store owners
- Educational content providers

**Goals:**
- Enable AI discoverability without compromising security
- Maintain server performance under bot traffic
- Control which content is accessible
- Understand how AI bots interact with their site
- Support modern page builders without breaking AI consumption

**Pain Points:**
- Technical complexity of API configuration
- Security concerns about exposing content
- Server resource management under bot load
- Lack of visibility into bot behavior
- Uncertainty about best practices
- Page builders create complex content structures

**Needs:**
- Simple, intuitive configuration interface
- Granular control over accessible content
- Usage analytics and logging
- Clear documentation and guidance
- Performance optimization built-in
- Page builder compatibility out-of-the-box

### 2.2.3 The WordPress Developer

**Profile:** Developer integrating or extending the plugin for custom requirements.

**Examples:**
- Agency developers building client sites
- Plugin developers creating integrations
- Enterprise developers with custom requirements
- Open source contributors

**Goals:**
- Customize Markdown output for specific needs
- Integrate with existing systems and workflows
- Extend functionality without modifying core
- Contribute improvements back to the project

**Pain Points:**
- Limited extensibility in existing solutions
- Poor or outdated documentation
- Breaking changes between versions
- Lack of testing utilities

**Needs:**
- Comprehensive hooks and filters
- Clear, up-to-date API documentation
- Semantic versioning with migration guides
- Testing utilities and examples
- Active community and support channels

---

# 3. Functional Requirements

## 3.1 Endpoint Architecture

The plugin must expose a clear endpoint structure supporting multiple access methods for flexibility and compatibility across different AI bot implementations.

### 3.1.1 Primary Access Methods

| Method | Format | Example | Primary Use Case |
|--------|--------|---------|------------------|
| **Query Parameter** | Append `?format=markdown` to any public URL | `https://example.com/about-us/?format=markdown` | Simple, works with any URL |
| **REST API by URL** | Access via dedicated REST endpoint | `GET /wp-json/wp-to-markdown/v1/content/?url={URL}` | Programmatic access |
| **REST API by ID** | Access via post/page ID | `GET /wp-json/wp-to-markdown/v1/content/{id}` | Direct ID-based access |

### 3.1.2 Discovery & Index Endpoints

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/wp-json/wp-to-markdown/v1/index/` | GET | List all available Markdown URLs (paginated) |
| `/wp-json/wp-to-markdown/v1/index/?type={post_type}` | GET | Filter index by post type |
| `/wp-json/wp-to-markdown/v1/index/?page={n}&per_page={n}` | GET | Paginated index results |
| `/wp-json/wp-to-markdown/v1/index/?modified_after={date}` | GET | Content modified after specified date |
| `/wp-json/wp-to-markdown/v1/search/?q={query}` | GET | Search available content |
| `/wp-json/wp-to-markdown/v1/status/` | GET | Health check and system information |
| `/.well-known/ai-markdown.json` | GET | Discovery specification (RFC 8615 well-known URI) |
| `/llms.txt` | GET | LLM-friendly site description |
| `/llms-full.txt` | GET | Complete content index for LLMs |

### 3.1.3 Request/Response Flow

**Standard Request Format:**

```
GET https://example.com/about-us/?format=markdown
Accept: text/markdown
Authorization: Bearer {api_key}
User-Agent: GPTBot/1.0
If-None-Match: "abc123def456"
If-Modified-Since: Mon, 15 Jan 2025 10:00:00 GMT
```

**Success Response (200 OK):**

```
HTTP/1.1 200 OK
Content-Type: text/markdown; charset=UTF-8
Cache-Control: public, max-age=86400, s-maxage=86400
ETag: "abc123def456"
Last-Modified: Mon, 15 Jan 2025 10:30:00 GMT
Vary: Accept, Authorization
X-Markdown-Generated: 2025-01-15T10:30:00Z
X-Content-Hash: sha256:abc123...
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1705320000
X-Cache-Status: HIT
```

**Cached Response (304 Not Modified):**

```
HTTP/1.1 304 Not Modified
ETag: "abc123def456"
Cache-Control: public, max-age=86400
X-Cache-Status: HIT
```

### 3.1.4 HTTP Status Codes

| Status Code | Scenario | Response Format |
|-------------|----------|-----------------|
| `200 OK` | Successful request | Markdown content with full headers |
| `304 Not Modified` | Content unchanged (conditional GET) | Empty body with ETag |
| `400 Bad Request` | Malformed URL or parameters | JSON error object |
| `401 Unauthorized` | Missing or invalid authentication | JSON error object |
| `403 Forbidden` | Content excluded from AI access | JSON error object |
| `404 Not Found` | Page doesn't exist, is draft, or is private | JSON error object |
| `410 Gone` | Content was permanently deleted | JSON error object |
| `413 Payload Too Large` | Content exceeds configured size limit | JSON error object |
| `429 Too Many Requests` | Rate limit exceeded | JSON error with Retry-After header |
| `500 Internal Server Error` | Conversion failure | JSON error object |
| `503 Service Unavailable` | Maintenance mode or overload | JSON error with Retry-After header |

**Error Response Format:**

```json
{
  "code": "rate_limit_exceeded",
  "message": "Too many requests. Please retry after 60 seconds.",
  "data": {
    "status": 429,
    "retry_after": 60,
    "limit": 60,
    "window": "1 minute"
  },
  "documentation": "https://example.com/wp-json/wp-to-markdown/v1/docs/"
}
```

## 3.2 Content Parsing & Cleanup Engine

The core engine must strip away structural web noise, isolate primary content, and intelligently handle page builder output.

### 3.2.1 HTML Elements to Remove

| Element Type | Tags/Selectors |
|--------------|----------------|
| **Navigation** | `<nav>`, `[role="navigation"]`, `.navigation`, `.nav-menu` |
| **Headers** | `<header>`, `[role="banner"]`, `.site-header` |
| **Footers** | `<footer>`, `[role="contentinfo"]`, `.site-footer` |
| **Sidebars** | `<aside>`, `<sidebar>`, `[role="complementary"]`, `.sidebar`, `.widget-area` |
| **Scripts & Styles** | `<script>`, `<noscript>`, `<style>`, `<link rel="stylesheet">` |
| **Comments** | HTML comments `<!-- -->`, comment sections |
| **Forms** | `<form>` (unless explicitly included) |
| **Advertisements** | Common ad container classes, `<ins>` tags |
| **Hidden Content** | `[hidden]`, `[aria-hidden="true"]`, `.hidden`, `.screen-reader-text` |
| **Custom Exclusions** | Admin-defined CSS classes and IDs |

### 3.2.2 HTML to Markdown Conversion Rules

| HTML Element | Markdown Output | Notes |
|--------------|-----------------|-------|
| `<h1>` through `<h6>` | `#` through `######` | Preserve heading hierarchy |
| `<p>` | Paragraph with blank line separation | Maintain readability |
| `<strong>`, `<b>` | `**text**` | Bold formatting |
| `<em>`, `<i>` | `*text*` | Italic formatting |
| `<del>`, `<s>`, `<strike>` | `~~text~~` | Strikethrough |
| `<code>` | `` `code` `` | Inline code |
| `<pre><code>` | Fenced code block with language | Preserve syntax highlighting hints |
| `<a href="">` | `[text](URL)` | Convert to absolute URLs |
| `<img>` | `![alt text](URL "title")` | Include alt text and title |
| `<ul>`, `<ol>` | `*` or `1.` | Preserve nesting depth |
| `<blockquote>` | `> text` | Block quotes |
| `<hr>` | `---` | Horizontal rule |
| `<table>` | Pipe table syntax | Simplified if complex (>6 columns) |
| `<br>` | Two trailing spaces + newline | Line break |
| `<sup>` | `^text^` | Superscript |
| `<sub>` | `~text~` | Subscript |
| `<details>`, `<summary>` | Expanded content | Convert to visible content |

### 3.2.3 Special Content Handling

| Content Type | Default Handling | Configurable |
|--------------|------------------|--------------|
| **Images** | Convert to `![alt](URL)` with full URL; optionally include dimensions in title | Yes |
| **Galleries** | Convert to list of image links or grid format | Yes |
| **YouTube Embeds** | Convert to `[Video: Title](URL)` or embed markdown | Yes |
| **Twitter/X Embeds** | Convert to quoted text with link to original | Yes |
| **Other oEmbeds** | Convert to `[Embedded Content: Type](URL)` | Yes |
| **Audio** | Convert to `[Audio: Title](URL)` | Yes |
| **Video** | Convert to `[Video: Title](URL)` | Yes |
| **iframes** | Strip or convert to link based on settings | Yes |
| **Shortcodes** | Render first, then convert output; option to strip | Yes |
| **Gutenberg Blocks** | Parse registered blocks; custom handling per block type | Yes |
| **Custom HTML Blocks** | Attempt conversion; strip if unparseable | Yes |
| **Code Blocks** | Preserve language identifier for syntax highlighting | No |
| **Tables** | Convert to pipe tables; simplify if >6 columns | Yes |
| **Math/LaTeX** | Preserve LaTeX notation within `$` or `$$` | No |
| **Footnotes** | Convert to Markdown footnote syntax `[^1]` | No |

### 3.2.4 Link Handling

| Link Type | Handling Strategy |
|-----------|-------------------|
| **Internal Links** | Convert to full absolute URLs |
| **Anchor Links** | Preserve as `#section-id` appended to full URL |
| **Relative URLs** | Convert to absolute URLs |
| **Email Links** | Preserve `mailto:` links |
| **Phone Links** | Preserve `tel:` links |
| **Download Links** | Include file type indicator `[Download PDF](URL)` |
| **Broken Links** | Preserve but optionally flag in metadata |
| **Affiliate Links** | Preserve with optional disclosure marker |

### 3.2.5 Metadata Front-Matter

Every generated Markdown document must include a YAML front-matter block containing structured metadata:

```yaml
---
title: "Page Title"
description: "Meta description or excerpt"
date_published: "2024-06-15T14:30:00Z"
date_modified: "2025-01-10T09:15:00Z"
author: "Author Name"
authors:
  - name: "Author Name"
    url: "https://example.com/author/john/"
permalink: "https://example.com/about-us/"
slug: "about-us"
type: "page"
status: "published"
featured_image: "https://example.com/wp-content/uploads/featured.jpg"
featured_image_alt: "Featured image description"
categories:
  - "Category Name"
tags:
  - "Tag 1"
  - "Tag 2"
language: "en-US"
word_count: 1250
reading_time: "5 min"
robots: "index, follow"
canonical_url: "https://example.com/about-us/"
builder: "elementor"
custom_fields:
  custom_key: "custom_value"
---
```

**Required Fields:**
- `title`
- `date_published`
- `date_modified`
- `permalink`

**Optional Fields (configurable):**
- All other fields based on admin settings

## 3.3 Admin Configuration Settings

The plugin provides a settings page under **Settings > AI Markdown Settings** with a tabbed interface for organized configuration.

### 3.3.1 General Settings

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| **Global Toggle** | Toggle | Enabled | Enable/Disable the Markdown endpoint site-wide |
| **Query Parameter** | Text | `markdown` | Customize the query parameter name (e.g., `?format=markdown`) |
| **REST API Prefix** | Text | `wp-to-markdown/v1` | Customize REST API namespace |
| **Default Response Format** | Select | `text/markdown` | Choose between `text/markdown` or `text/plain` |

### 3.3.2 Content Settings

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| **Post Type Selection** | Checkboxes | Posts, Pages | Enable for specific built-in post types |
| **Custom Post Types** | Multi-select | None | Select from registered CPTs |
| **Include Password-Protected** | Toggle | Disabled | Allow access to password-protected content with valid password |
| **Include Private Posts** | Toggle | Disabled | Allow access to private posts (requires authentication) |
| **Content Length Limit** | Number | 0 (unlimited) | Maximum characters in output (0 = unlimited) |
| **Include Excerpt** | Toggle | Enabled | Include excerpt in front-matter |
| **Include Featured Image** | Toggle | Enabled | Include featured image URL in front-matter |
| **Include Author Info** | Toggle | Enabled | Include author details in front-matter |
| **Include Taxonomies** | Toggle | Enabled | Include categories/tags in front-matter |
| **Custom Fields** | Text (comma-separated) | Empty | List of custom field keys to include |
| **ACF Fields** | Multi-select | None | Select ACF fields to include (if ACF active) |

### 3.3.3 Exclusions

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| **Excluded Post IDs** | Textarea | Empty | Comma-separated list of post IDs to exclude |
| **Excluded URLs** | Textarea | Empty | One URL pattern per line (supports wildcards) |
| **Excluded Categories** | Multi-select | None | Categories to exclude entirely |
| **Excluded Tags** | Multi-select | None | Tags to exclude entirely |
| **Excluded CSS Classes** | Text | `.no-ai-access, .private-content` | Content with these classes will be stripped |
| **Excluded HTML Elements** | Text | Empty | Additional HTML elements/selectors to strip |
| **Excluded Shortcodes** | Text | Empty | Shortcodes to strip rather than render |

### 3.3.4 Security Settings

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| **Access Mode** | Select | Public | Public, Restricted (API Key), or Whitelist (User-Agent) |
| **API Keys** | Key Manager | Empty | Generate, view, revoke API keys |
| **API Key Header Name** | Text | `Authorization` | Header name for API key (e.g., `X-API-Key`) |
| **Allowed User-Agents** | Textarea | GPTBot, ClaudeBot, PerplexityBot | Allowed bot User-Agents (one per line) |
| **IP Whitelist** | Textarea | Empty | IP addresses/ranges to always allow |
| **IP Blacklist** | Textarea | Empty | IP addresses/ranges to always block |
| **Rate Limit** | Number | 60 | Maximum requests per minute per IP |
| **Rate Limit Window** | Number | 60 | Time window in seconds |
| **Rate Limit by** | Select | IP | Rate limit by IP, API Key, or User-Agent |
| **CORS Origins** | Textarea | `*` | Allowed CORS origins |

### 3.3.5 Performance Settings

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| **Enable Caching** | Toggle | Enabled | Cache generated Markdown output |
| **Cache TTL** | Number | 86400 | Cache duration in seconds (default: 24 hours) |
| **Cache Storage** | Select | Auto-detect | Transients, Object Cache, or File-based |
| **Warm Cache on Publish** | Toggle | Disabled | Pre-generate cache when content is published |
| **Cache Preload** | Button | - | Manually trigger cache generation for all content |
| **Clear All Cache** | Button | - | Purge all cached Markdown content |

### 3.3.6 Advanced Settings

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| **Image Handling** | Select | Include | Include, Link Only, or Exclude images |
| **Image URL Format** | Select | Full URL | Full URL, Relative, or CDN URL |
| **Embed Handling** | Select | Convert to Link | Convert to Link, Include, or Exclude |
| **Table Handling** | Select | Pipe Tables | Pipe Tables, Simple List, or HTML |
| **Link URL Format** | Select | Absolute | Absolute or Relative URLs |
| **Shortcode Processing** | Select | Render | Render, Strip, or Preserve as text |
| **HTML Fallback** | Toggle | Disabled | Include raw HTML for unconvertible elements |
| **Debug Mode** | Toggle | Disabled | Enable verbose logging for troubleshooting |

### 3.3.7 Logging & Analytics

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| **Enable Logging** | Toggle | Enabled | Log API requests |
| **Log Retention** | Number | 30 | Days to retain logs |
| **Log Detail Level** | Select | Standard | Minimal, Standard, or Verbose |
| **Dashboard Widget** | Toggle | Enabled | Show analytics widget on WordPress dashboard |

**Analytics Dashboard Includes:**
- Total requests (today, week, month, all-time)
- Requests by User-Agent (visual chart)
- Top requested pages (sortable table)
- Response code distribution (pie chart)
- Cache hit rate percentage
- Average response time (milliseconds)
- CSV export functionality

### 3.3.8 Integration Settings

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| **robots.txt Integration** | Toggle | Disabled | Auto-add rules for AI crawlers |
| **robots.txt Rules** | Textarea | Pre-populated | Custom robots.txt rules to add |
| **Sitemap Integration** | Toggle | Disabled | Add Markdown URLs to XML sitemap |
| **llms.txt Support** | Toggle | Disabled | Generate `/llms.txt` file |
| **llms-full.txt Support** | Toggle | Disabled | Generate `/llms-full.txt` file |

### 3.3.9 Discovery Settings

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| **robots.txt AI Directives** | Toggle | Enabled | Add AI-Markdown-* directives to robots.txt |
| **Well-Known Endpoint** | Toggle | Enabled | Serve `/.well-known/ai-markdown.json` |
| **HTTP Headers** | Toggle | Enabled | Add `X-AI-Markdown-*` headers to responses |
| **HTML Meta Tags** | Toggle | Enabled | Add meta tags to HTML head |
| **Site Description (llms.txt)** | Textarea | Empty | Custom description for AI bots |
| **AI Contact Email** | Email | Admin email | Contact for AI-related inquiries |
| **Terms Summary** | Textarea | Empty | Brief terms of use for AI consumption |
| **Commercial Use Policy** | Select | Contact Required | Allowed, Allowed with Attribution, Contact Required, Not Allowed |

---

# 4. Non-Functional Requirements

## 4.1 Performance

| Requirement | Target | Measurement Method |
|-------------|--------|-------------------|
| **Cached Response Time** | < 100ms (p95) | Server response time monitoring |
| **Uncached Response Time** | < 500ms (p95) | Server response time monitoring |
| **Memory Usage** | < 64MB per request | Peak memory consumption profiling |
| **Cache Hit Rate** | > 90% | Hits / Total requests ratio |
| **Concurrent Requests** | 100+ simultaneous | Load testing with k6/Locust |
| **Database Queries** | < 10 per request | Query Monitor plugin |

## 4.2 Caching Strategy

### 4.2.1 Cache Storage Options

| Method | When to Use | Advantages | Limitations |
|--------|-------------|------------|-------------|
| **WordPress Transients** | Default/fallback | Universal compatibility | Database-dependent |
| **Object Cache** | Redis/Memcached available | Fastest performance | Requires server setup |
| **File Cache** | High-traffic sites | Reduces database load | File system dependent |

### 4.2.2 Cache Invalidation Triggers

| Trigger | Action |
|---------|--------|
| Post/page updated | Clear cache for that specific content |
| Post/page deleted | Remove cache entry |
| Post/page trashed | Clear cache entry |
| Bulk edit operation | Clear all affected entries |
| Settings changed | Option to clear all or keep existing |
| Manual purge | Clear all or specific URL |
| Cache TTL expired | Automatic removal |
| Page builder data changed | Clear builder-specific cache |

### 4.2.3 Cache Headers

The plugin must set appropriate HTTP cache headers:

```
Cache-Control: public, max-age=86400, s-maxage=86400
ETag: "content-hash-abc123"
Last-Modified: Mon, 15 Jan 2025 10:30:00 GMT
Vary: Accept, Authorization
X-Cache-Status: HIT|MISS|BYPASS
```

## 4.3 Scalability

| Aspect | Implementation Strategy |
|--------|------------------------|
| **CDN Compatibility** | Proper cache headers, Vary header, cache keys for CDN edge caching |
| **Load Balancer Support** | Stateless design, no session dependency |
| **Database Optimization** | Indexed queries, pagination, query limits |
| **Bulk Operations** | Chunked processing, background jobs for large sites |
| **Page Builder Extraction** | Cached separately due to computational expense |

## 4.4 Reliability

| Requirement | Implementation |
|-------------|----------------|
| **Graceful Degradation** | Return basic content if conversion fails |
| **Error Recovery** | Automatic retry for transient failures |
| **Timeout Handling** | 30-second maximum processing time with fallback |
| **Health Monitoring** | Status endpoint for uptime checks |
| **Backup Strategy** | Fallback to HTML if Markdown conversion fails |

## 4.5 Internationalization (i18n)

| Requirement | Implementation |
|-------------|----------------|
| **Admin Strings** | All UI text translatable via standard WordPress functions |
| **Translation Files** | Include `.pot` template file in plugin |
| **RTL Support** | Admin UI supports right-to-left languages |
| **Content Language** | Include `language` code in YAML front-matter |
| **Character Encoding** | Full UTF-8 support including emoji |
| **Multi-language Plugins** | WPML, Polylang, TranslatePress compatibility |

## 4.6 Accessibility

| Requirement | Implementation |
|-------------|----------------|
| **Admin Interface** | WCAG 2.1 AA compliant |
| **Keyboard Navigation** | Full keyboard support in all settings screens |
| **Screen Reader Support** | Proper ARIA labels and roles throughout admin UI |
| **Color Contrast** | Sufficient contrast ratios for all text |

---

# 5. Security Requirements

## 5.1 Authentication & Authorization

| Mechanism | Implementation |
|-----------|----------------|
| **API Key Authentication** | Bearer token in `Authorization` header (RFC 6750) |
| **Multiple API Keys** | Support for multiple keys with descriptive labels |
| **Key Rotation** | Ability to regenerate keys without downtime |
| **Key Expiration** | Optional expiration dates for time-limited keys |
| **Capability Checks** | `manage_options` capability required for admin settings |
| **Nonce Verification** | Required for all admin AJAX operations |

## 5.2 Rate Limiting

| Feature | Implementation |
|---------|----------------|
| **Request Throttling** | Configurable requests per minute per identifier |
| **Sliding Window** | Rate limit window resets progressively |
| **Bypass for Authenticated** | Higher limits for requests with valid API keys |
| **Response Headers** | Include standard `X-RateLimit-*` headers |
| **429 Response** | Include `Retry-After` header per RFC 6585 |

**Rate Limit Response Headers:**

```
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1705320000
Retry-After: 60
```

## 5.3 Input Validation & Sanitization

| Input Type | Validation Method |
|------------|-------------------|
| **URL Parameters** | `esc_url()`, protocol validation, domain verification |
| **Post IDs** | `absint()`, existence check |
| **API Keys** | Alphanumeric validation, minimum length check |
| **Admin Settings** | Type-specific sanitization callbacks |
| **Search Queries** | `sanitize_text_field()`, maximum length limit |
| **IP Addresses** | Proper IP validation, CIDR range support |

## 5.4 Output Security

| Measure | Implementation |
|---------|----------------|
| **Content Escaping** | Escape all user-generated content in output |
| **Header Injection Prevention** | Validate all header values before sending |
| **JSON Encoding** | Use `JSON_HEX_TAG` and `JSON_HEX_AMP` flags |
| **No Sensitive Data** | Never expose passwords, emails, private meta data |

## 5.5 Security Headers

The plugin must set appropriate security headers:

```
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-Robots-Tag: noindex (for error responses only)
Referrer-Policy: strict-origin-when-cross-origin
```

## 5.6 Additional Security Measures

| Measure | Implementation |
|---------|----------------|
| **IP Allowlist/Blocklist** | Admin-configurable lists with CIDR support |
| **User-Agent Filtering** | Optional whitelist mode for known AI bots |
| **Honeypot Detection** | Hidden endpoint to detect malicious scraping patterns |
| **Audit Logging** | Log all authentication failures and access denials |
| **Brute Force Protection** | Temporary lockout after repeated auth failures |

---

# 6. Technical Stack & Implementation

## 6.1 System Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **WordPress** | 6.0 | 6.4+ |
| **PHP** | 8.1 | 8.2+ |
| **MySQL** | 5.7 | 8.0+ |
| **Memory Limit** | 128MB | 256MB+ |
| **Web Server** | Apache 2.4 or Nginx 1.18+ | Latest stable versions |

## 6.2 Dependencies

| Dependency | Purpose | Integration |
|------------|---------|-------------|
| **league/html-to-markdown** | HTML to Markdown conversion library | Bundled (optional) |
| **Native WordPress Functions** | Alternative conversion using WP core | Built-in |

**Dependency Strategy:**
- Primary: Use native WordPress parsing for simple content structures
- Fallback: Use bundled library for complex HTML structures (page builder content)
- No external API calls required
- Optional dependencies loaded only when needed

## 6.3 WordPress Integration

### 6.3.1 Hooks System

The plugin must provide comprehensive hooks for extensibility:

**Actions (Event-based):**
- Plugin lifecycle events (activated, deactivated, upgraded)
- Request lifecycle events (before/after response)
- Conversion lifecycle events (before/after conversion, failure)
- Cache lifecycle events (set, cleared)
- Security events (rate limit, auth failures, access denied)

**Filters (Content-based):**
- Content modification (markdown output, front-matter, custom fields)
- Element handling (excluded tags, classes, shortcodes)
- Configuration (allowed post types, rate limits, cache TTL)
- Security (allowed user-agents, IPs)
- Response modification (headers, error responses)

### 6.3.2 Database Usage

**No custom database tables required.** The plugin uses standard WordPress storage:

| Storage Type | Purpose |
|--------------|---------|
| `wp_options` | Plugin settings, API keys (encrypted), version tracking |
| `wp_transients` | Default cache storage |
| `wp_postmeta` | Per-post exclusion flags, page builder metadata |
| `wp_usermeta` | User-specific settings (if needed) |

**Option Keys:**
- `wp_to_markdown_settings` - Main settings array
- `wp_to_markdown_api_keys` - Encrypted API keys
- `wp_to_markdown_version` - Installed version for migrations

### 6.3.3 File Structure

```
wp-to-markdown/
├── wp-to-markdown.php              Main plugin bootstrap file
├── readme.txt                       WordPress.org readme format
├── LICENSE                          GPL v2 or later license
├── CHANGELOG.md                     Version history
├── composer.json                    Dependency configuration
├── uninstall.php                    Cleanup on uninstall
│
├── includes/                        Core functionality
│   ├── class-plugin.php            Main plugin orchestrator
│   ├── class-converter.php         HTML to Markdown conversion engine
│   ├── class-page-builder-detector.php    Detects page builders in use
│   ├── class-page-builder-extractor.php   Builder-specific extraction
│   ├── class-cache.php             Cache management (multi-backend)
│   ├── class-security.php          Authentication & rate limiting
│   ├── class-rest-api.php          REST API endpoint handlers
│   ├── class-logger.php            Request logging & analytics
│   ├── class-settings.php          Settings registration & sanitization
│   ├── class-discovery.php         Discovery protocol implementation
│   └── functions.php               Helper functions
│
├── admin/                           Admin interface
│   ├── class-admin.php             Admin functionality coordinator
│   ├── class-settings-page.php     Settings page UI with tabs
│   ├── class-dashboard-widget.php  Analytics dashboard widget
│   ├── css/
│   │   └── admin.css               Admin styling
│   └── js/
│       └── admin.js                Admin interactivity (tabs, etc.)
│
├── languages/                       Internationalization
│   └── wp-to-markdown.pot          Translation template
│
└── tests/                           Automated tests
    ├── bootstrap.php               Test environment setup
    ├── unit/                       Unit tests (PHPUnit)
    ├── integration/                Integration tests
    └── fixtures/                   Test data and content samples
```

## 6.4 Coding Standards

| Standard | Requirement |
|----------|-------------|
| **PHP** | WordPress PHP Coding Standards (WPCS) |
| **JavaScript** | WordPress JavaScript Coding Standards |
| **CSS** | WordPress CSS Coding Standards |
| **Documentation** | PHPDoc blocks for all public methods and classes |
| **Naming Conventions** | Prefix all functions and classes with `wp_to_markdown_` or `WP_To_Markdown_` |
| **Security** | Always escape output, sanitize input, verify nonces |

---

# 7. Page Builder Compatibility

## 7.1 Overview

Modern WordPress sites extensively use page builders which generate complex, nested HTML structures that simple regex-based parsing cannot handle accurately. The plugin must intelligently detect and extract content from all major page builders.

## 7.2 Supported Page Builders

| Page Builder | Detection Method | Extraction Strategy |
|--------------|------------------|---------------------|
| **Elementor** | Plugin constant, data attributes, post meta | Use structured JSON data from `_elementor_data` meta |
| **Divi** | Plugin constant, shortcode patterns, post meta | Parse `[et_pb_*]` shortcodes intelligently |
| **WPBakery** | Plugin constant, `vc_*` classes, post meta | Parse nested shortcode structure |
| **Beaver Builder** | Plugin class, `fl-*` CSS classes | Parse rendered HTML structure |
| **Oxygen Builder** | Plugin constant, `ct-section` classes | Parse shortcode-based storage |
| **Bricks Builder** | Plugin constant, `brxe-` classes | Parse rendered HTML |
| **Gutenberg** | Block comments in content | Parse block structure natively |
| **Standard Editor** | Default fallback | Standard HTML parsing |

## 7.3 Detection Process

```
┌─────────────────────────────────────────┐
│    1. Check for page builder constants  │
│    2. Analyze post content for markers  │
│    3. Check post meta for builder data  │
│    4. Identify primary builder in use   │
└─────────────────────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────┐
│      Route to builder-specific          │
│      extraction strategy                │
└─────────────────────────────────────────┘
```

## 7.4 Extraction Strategies

### 7.4.1 Elementor

**Primary Method:** Use structured data from `_elementor_data` post meta (JSON format)

**Process:**
1. Decode JSON data structure
2. Iterate through elements recursively
3. Identify widget types and extract relevant content
4. Handle nested sections, columns, and inner sections
5. Map widget-specific fields to Markdown equivalents

**Supported Widgets:**
- heading, text-editor, image, button, icon-box
- accordion, tabs, counter, progress, testimonial
- price-list, price-table, form (metadata only), shortcode, html
- Plus intelligent text extraction from unknown widgets

**Fallback Method:** Parse rendered HTML if structured data unavailable

### 7.4.2 Divi

**Primary Method:** Parse `[et_pb_*]` shortcode structure

**Process:**
1. Identify section boundaries
2. Extract content from individual modules
3. Map Divi modules to Markdown equivalents
4. Handle nested module structures

**Supported Modules:**
- et_pb_text, et_pb_blurb, et_pb_cta, et_pb_button
- et_pb_code, et_pb_heading, et_pb_pricing_tables
- et_pb_tabs, et_pb_tab, et_pb_accordion, et_pb_toggle
- et_pb_testimonial, et_pb_slider, et_pb_slide
- et_pb_portfolio, et_pb_gallery, et_pb_image
- et_pb_counters, et_pb_counter, et_pb_number_counter
- et_pb_team_member, et_pb_blog, et_pb_post_title
- et_pb_post_content, et_pb_contact_form, et_pb_signup

**Fallback Method:** Clean rendered HTML

### 7.4.3 WPBakery (Visual Composer)

**Primary Method:** Parse nested shortcode structure

**Process:**
1. Identify top-level containers (vc_row, vc_column)
2. Extract content from individual widgets
3. Handle shortcode attributes and nested content
4. Map widget outputs to Markdown

**Supported Widgets:**
- vc_text_block, vc_heading, vc_btn, vc_single_image
- vc_cta, vc_btn_with_icon, vc_icon, vc_column_text
- vc_accordion, vc_accordion_tab, vc_tabs, vc_tab
- vc_pie, vc_progress_bar, vc_single_bar, vc_message
- vc_quote, vc_testimonial, vc_pricing_table
- vc_video, vc_youtube, vc_vimeo, vc_gmaps

**Fallback Method:** Clean rendered HTML

### 7.4.4 Beaver Builder

**Primary Method:** Parse rendered HTML structure

**Process:**
1. Identify module boundaries using `fl-module-*` classes
2. Extract content from each module type
3. Clean up structural elements
4. Convert to Markdown

**Supported Modules:**
- heading, rich-text, text, button
- Plus structural elements (rows, columns)

### 7.4.5 Oxygen Builder

**Primary Method:** Parse shortcode wrapper structure

**Process:**
1. Remove Oxygen wrapper tags
2. Extract inner content
3. Clean and convert to Markdown

### 7.4.6 Bricks Builder

**Primary Method:** Parse rendered HTML with data attribute cleanup

**Process:**
1. Remove Bricks-specific data attributes
2. Clean structural elements
3. Convert to Markdown

### 7.4.7 Gutenberg (Block Editor)

**Primary Method:** Use WordPress block parsing functions

**Process:**
1. Parse blocks using `parse_blocks()`
2. Render each block to Markdown based on block type
3. Handle nested blocks recursively
4. Preserve block-specific formatting

**Supported Core Blocks:**
- core/heading, core/paragraph, core/list
- core/quote, core/pullquote, core/image
- core/gallery, core/table, core/columns
- core/column, plus intelligent fallback for unknown blocks

## 7.5 Performance Considerations

| Concern | Mitigation Strategy |
|---------|---------------------|
| **Computational Expense** | Page builder extraction results are cached separately |
| **Memory Usage** | Stream-based processing for large content |
| **Processing Time** | 30-second timeout with fallback to basic conversion |
| **Cache Invalidation** | Builder-specific cache cleared on post update |

## 7.6 Admin Configuration for Page Builders

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| **Use Builder API** | Toggle | Enabled | Use page builder structured data when available |
| **Builder-Specific Processing** | Toggle | Enabled | Apply builder-specific extraction logic |
| **Fallback to HTML** | Toggle | Enabled | Fall back to HTML parsing if builder extraction fails |
| **Cache Builder Data** | Toggle | Enabled | Cache extracted builder data separately |

---

# 8. AI Markdown Discovery Protocol

## 8.1 Purpose

Define a standardized method for websites to advertise their AI-friendly Markdown endpoints to crawler bots, enabling automatic discovery without requiring prior knowledge of specific plugin implementations.

## 8.2 Discovery Methods

| Method | Priority | Description |
|--------|----------|-------------|
| **robots.txt Extension** | Primary | Universal, checked by all bots first |
| **Well-Known URI** | Secondary | REST-friendly JSON configuration |
| **HTTP Headers** | Supplementary | Available on every response |
| **HTML Meta Tags** | Fallback | For HTML-parsing bots |

## 8.3 robots.txt Extension

### 8.3.1 New Directives

| Directive | Purpose | Format |
|-----------|---------|--------|
| `AI-Markdown` | Declare Markdown support | `AI-Markdown: true` |
| `AI-Markdown-Version` | Protocol version | `AI-Markdown-Version: 1.0` |
| `AI-Markdown-Endpoint` | REST API endpoint | `AI-Markdown-Endpoint: /wp-json/wp-to-markdown/v1/` |
| `AI-Markdown-Param` | Query parameter method | `AI-Markdown-Param: format=markdown` |
| `AI-Markdown-Index` | Content discovery URL | `AI-Markdown-Index: /wp-json/wp-to-markdown/v1/index/` |
| `AI-Markdown-Auth` | Authentication method | `AI-Markdown-Auth: none\|bearer\|api-key` |
| `AI-Markdown-Spec` | Full specification URL | `AI-Markdown-Spec: /.well-known/ai-markdown.json` |
| `AI-Markdown-Rate-Limit` | Rate limit info | `AI-Markdown-Rate-Limit: 60/minute` |

### 8.3.2 Example robots.txt Output

```
# Standard robots.txt directives
User-agent: *
Disallow: /wp-admin/
Allow: /wp-admin/admin-ajax.php

# Sitemap reference
Sitemap: https://example.com/sitemap.xml

# ============================================
# AI Markdown Protocol v1.0
# ============================================
AI-Markdown: true
AI-Markdown-Version: 1.0
AI-Markdown-Endpoint: /wp-json/wp-to-markdown/v1/content/
AI-Markdown-Param: format=markdown
AI-Markdown-Index: /wp-json/wp-to-markdown/v1/index/
AI-Markdown-Auth: none
AI-Markdown-Rate-Limit: 60/minute
AI-Markdown-Spec: /.well-known/ai-markdown.json

# AI Bot Permissions
User-agent: GPTBot
Allow: /*?format=markdown
Allow: /wp-json/wp-to-markdown/
Crawl-delay: 1

User-agent: ClaudeBot
Allow: /*?format=markdown
Allow: /wp-json/wp-to-markdown/
Crawl-delay: 1

User-agent: PerplexityBot
Allow: /*?format=markdown
Allow: /wp-json/wp-to-markdown/
Crawl-delay: 1
```

## 8.4 Well-Known URI Specification

The plugin serves a standardized JSON specification file at `/.well-known/ai-markdown.json` following RFC 8615.

### 8.4.1 JSON Schema

```json
{
  "$schema": "https://ai-markdown-protocol.org/schema/1.0.json",
  "ai_markdown": {
    "version": "1.0",
    "enabled": true,
    "generator": "WP-to-Markdown/1.0",
    "generated_at": "2025-01-15T10:30:00Z",
    "endpoints": {
      "content": {
        "url": "/wp-json/wp-to-markdown/v1/content/{id}",
        "method": "GET",
        "description": "Retrieve content by ID"
      },
      "content_by_url": {
        "url": "/wp-json/wp-to-markdown/v1/content/",
        "method": "GET",
        "parameters": {
          "url": {"type": "string", "required": true}
        }
      },
      "query_param": {
        "parameter": "format",
        "value": "markdown",
        "example": "https://example.com/page/?format=markdown"
      },
      "index": {
        "url": "/wp-json/wp-to-markdown/v1/index/",
        "method": "GET"
      },
      "search": {
        "url": "/wp-json/wp-to-markdown/v1/search/",
        "method": "GET",
        "parameters": {
          "q": {"type": "string", "required": true}
        }
      }
    },
    "authentication": {
      "required": false,
      "methods": ["none"]
    },
    "rate_limiting": {
      "enabled": true,
      "requests_per_minute": 60
    },
    "content": {
      "types_available": ["post", "page"],
      "total_items": 156,
      "languages": ["en-US"],
      "last_updated": "2025-01-15T09:00:00Z"
    },
    "terms_of_use": {
      "url": "https://example.com/ai-terms/",
      "summary": "Content may be used with attribution.",
      "contact": "ai@example.com"
    }
  }
}
```

## 8.5 HTTP Headers

The plugin adds discovery headers to relevant HTTP responses:

```
X-AI-Markdown: true
X-AI-Markdown-URL: https://example.com/current-page/?format=markdown
X-AI-Markdown-Spec: https://example.com/.well-known/ai-markdown.json
Link: <https://example.com/page/?format=markdown>; rel="alternate"; type="text/markdown"
```

## 8.6 HTML Meta Tags

For crawlers that parse HTML, meta tags are added to the `<head>` section:

```html
<head>
    <meta name="ai-markdown" content="true">
    <meta name="ai-markdown-endpoint" content="/wp-json/wp-to-markdown/v1/content/">
    <meta name="ai-markdown-spec" content="/.well-known/ai-markdown.json">
    <link rel="alternate" type="text/markdown" href="https://example.com/page/?format=markdown">
</head>
```

## 8.7 llms.txt Integration

### 8.7.1 llms.txt Format (Basic)

```markdown
# Example.com

> Brief description of the website and its purpose.

## Content Access

This site supports AI-optimized Markdown responses.

- **Specification**: https://example.com/.well-known/ai-markdown.json
- **Content Index**: https://example.com/wp-json/wp-to-markdown/v1/index/
- **Query Parameter**: Append `?format=markdown` to any page URL

## Main Sections

- [Blog](/blog/): Latest articles and tutorials
- [Documentation](/docs/): Technical reference
- [About](/about/): Company information

## Terms of Use

Content may be used for AI training with attribution.
Contact: ai@example.com
```

### 8.7.2 llms-full.txt Format (Complete Index)

Contains complete metadata for all available content including URLs, summaries, and last-modified dates.

## 8.8 Bot Discovery Flow

```
1. Fetch robots.txt (always first)
2. Check for AI-Markdown: true directive
3. Parse AI-Markdown-* directives if present
4. Fetch /.well-known/ai-markdown.json for full configuration
5. Fall back to HTTP headers if above methods fail
6. Cache discovery results for efficiency (24 hours)
```

---

# 9. Testing Requirements

## 9.1 Unit Testing

| Test Category | Coverage Target | Tools |
|---------------|-----------------|-------|
| Converter Class | 100% | PHPUnit |
| Page Builder Detector | 100% | PHPUnit |
| Page Builder Extractor | 95% | PHPUnit |
| Cache Class | 100% | PHPUnit |
| Security Class | 100% | PHPUnit |
| REST API Class | 90% | PHPUnit |
| Settings Class | 90% | PHPUnit |
| Discovery Class | 90% | PHPUnit |
| Helper Functions | 100% | PHPUnit |

## 9.2 Integration Testing

| Test Category | Scope |
|---------------|-------|
| REST API Endpoints | All routes with various parameters |
| WordPress Hook Integration | Action and filter execution |
| Query Parameter Handling | HTTP request simulation |
| Database Operations | CRUD operations and migrations |
| Cache Integration | Transients and object cache |
| Page Builder Compatibility | Elementor, Divi, WPBakery, Beaver, Oxygen, Bricks |

## 9.3 Compatibility Testing

### 9.3.1 WordPress Versions

| Version | Test Type |
|---------|-----------|
| 6.0 | Full test suite |
| 6.1 | Full test suite |
| 6.2 | Full test suite |
| 6.3 | Full test suite |
| 6.4 | Full test suite |
| 6.5 (latest) | Full test suite |
| 6.6 (beta) | Smoke test |

### 9.3.2 PHP Versions

| Version | Test Type |
|---------|-----------|
| 8.1 | Full test suite |
| 8.2 | Full test suite |
| 8.3 | Full test suite |

### 9.3.3 Plugin Compatibility

| Plugin | Test Scope |
|--------|------------|
| Yoast SEO | Meta data extraction, sitemap integration |
| Rank Math | Meta data extraction |
| ACF (Advanced Custom Fields) | Custom field extraction |
| WooCommerce | Product content conversion |
| Elementor | Full content extraction |
| Divi | Full content extraction |
| WPBakery | Full content extraction |
| Beaver Builder | Full content extraction |
| Oxygen Builder | Content extraction |
| Bricks Builder | Content extraction |
| Gutenberg | Block parsing |
| WPML | Multi-language support |
| Polylang | Multi-language support |
| WP Super Cache | Cache compatibility |
| W3 Total Cache | Cache compatibility |
| WP Rocket | Cache compatibility |
| Wordfence | Security compatibility |
| Sucuri | Security compatibility |

### 9.3.4 Hosting Environments

| Environment | Test Focus |
|-------------|------------|
| Shared Hosting | Performance on limited resources |
| VPS | Standard functionality |
| WP Engine | Managed WP compatibility |
| Kinsta | Managed WP compatibility |
| Pantheon | Managed WP compatibility |
| Cloudways | Cloud hosting compatibility |
| Local (wp-env) | Development environment |

## 9.4 Performance Testing

| Test | Target | Tool |
|------|--------|------|
| Response Time (cached) | < 100ms p95 | Apache Bench, k6 |
| Response Time (uncached) | < 500ms p95 | Apache Bench, k6 |
| Concurrent Users | 100+ simultaneous | k6, Locust |
| Memory Usage | < 64MB per request | Xdebug profiler |
| Database Queries | < 10 per request | Query Monitor |
| Cache Efficiency | > 90% hit rate | Custom logging |
| Page Builder Extraction | < 1s per page | Performance profiling |

## 9.5 Security Testing

| Test | Method |
|------|--------|
| SQL Injection | Automated scanning, manual testing |
| XSS (Cross-Site Scripting) | Automated scanning, manual testing |
| CSRF (Cross-Site Request Forgery) | Manual nonce verification |
| Authentication Bypass | Manual penetration testing |
| Rate Limit Bypass | Automated testing |
| Information Disclosure | Manual response review |
| API Key Brute Force | Automated testing |

## 9.6 Automated Testing Pipeline

Continuous integration pipeline must run on every commit:

1. Code quality checks (PHPCS, PHPStan)
2. Unit tests across PHP versions
3. Integration tests with WordPress
4. Page builder compatibility tests
5. Security scans
6. Performance benchmarks

---

# 10. Documentation Requirements

## 10.1 User Documentation

| Document | Description | Format |
|----------|-------------|--------|
| README.md | GitHub repository overview | Markdown |
| readme.txt | WordPress.org plugin page | WordPress format |
| Installation Guide | Step-by-step installation | Markdown/Web |
| Quick Start Guide | 5-minute setup guide | Markdown/Web |
| Configuration Guide | Detailed settings explanations | Markdown/Web |
| FAQ | Frequently asked questions | Markdown/Web |
| Troubleshooting Guide | Common issues and solutions | Markdown/Web |

## 10.2 Developer Documentation

| Document | Description | Format |
|----------|-------------|--------|
| API Reference | Full REST API documentation | OpenAPI/Swagger |
| Hooks Reference | All actions and filters with examples | Markdown/Web |
| Code Examples | Common customization examples | Markdown/Web |
| Contributing Guide | How to contribute | Markdown |
| Architecture Overview | Technical design documentation | Markdown |
| Page Builder Integration | How to add new builders | Markdown |

## 10.3 Inline Documentation Standards

| Requirement | Standard |
|-------------|----------|
| File Headers | Plugin header with description, version, @since tags |
| Class Documentation | Class purpose, @since, @package |
| Method Documentation | Description, @param, @return, @since, @throws |
| Hook Documentation | Purpose, parameters, example usage |
| Complex Logic | Inline comments explaining rationale |

---

# 11. Deployment & Distribution

## 11.1 WordPress.org Submission Requirements

| Requirement | Status |
|-------------|--------|
| GPL v2 or later License | Required |
| No Tracking Without Consent | Required |
| No External Calls Without Disclosure | Required |
| Proper readme.txt Format | Required |
| Accurate Stable Tag | Required |
| No Ads in Admin | Required |
| Proper Sanitization/Escaping | Required |
| Nonce Verification | Required |
| No Premature Database Creation | Required |

## 11.2 Versioning Strategy

Follow Semantic Versioning (semver.org):

| Version Component | When to Increment |
|-------------------|-------------------|
| Major (X.0.0) | Breaking changes, major rewrites |
| Minor (0.X.0) | New features, non-breaking changes |
| Patch (0.0.X) | Bug fixes, security patches |

## 11.3 Release Process

1. Development on feature branch
2. Code review via pull request
3. Automated test suite execution
4. Manual QA testing in staging
5. Version number update
6. Changelog entry creation
7. Git tag with version
8. Distribution package build
9. WordPress.org SVN commit
10. GitHub release creation
11. Public announcement

## 11.4 Plugin Lifecycle Management

### 11.4.1 Activation

On plugin activation:
- Set default options
- Schedule cron jobs if needed
- Flush rewrite rules
- Create necessary database entries

### 11.4.2 Deactivation

On plugin deactivation:
- Clear scheduled cron jobs
- Optionally clear cache
- Flush rewrite rules
- Preserve user settings and API keys

### 11.4.3 Uninstall

On plugin uninstall (with admin confirmation):
- Delete all options
- Delete all transients
- Drop log tables
- Delete post meta
- Clear scheduled events

### 11.4.4 Updates

Update mechanism must:
- Use standard WordPress update API
- Run version migrations if needed
- Preserve settings across updates
- Optionally preserve or clear cache

---

# 12. Success Metrics

## 12.1 Technical Performance Metrics

| Metric | Target | Measurement Method |
|--------|--------|-------------------|
| Response Time (cached) | < 100ms p95 | Server logs, monitoring |
| Response Time (uncached) | < 500ms p95 | Server logs, monitoring |
| Cache Hit Rate | > 90% | Plugin analytics |
| Error Rate | < 0.1% | Plugin logging |
| Uptime | > 99.9% | External monitoring |
| Memory Usage | < 64MB p95 | Profiling |
| Page Builder Accuracy | > 95% | Manual QA testing |

## 12.2 Adoption Metrics

| Metric | Target (6 months) | Target (12 months) |
|--------|-------------------|-------------------|
| Active Installs | 1,000+ | 5,000+ |
| WordPress.org Rating | > 4.5 stars | > 4.5 stars |
| GitHub Stars | 100+ | 500+ |
| Support Tickets (unresolved) | < 5% | < 5% |
| Weekly Downloads | 200+ | 500+ |

## 12.3 Engagement Metrics

| Metric | Target |
|--------|--------|
| Documentation Page Views | 500+/month |
| API Requests (aggregate users) | 1M+/month |
| Community Contributions | 5+ PRs accepted |
| Translations | 5+ languages |
| Discovery Protocol Adoption | Tracked via robots.txt scans |

---

# 13. Acceptance Criteria

## 13.1 Minimum Viable Product (MVP)

The MVP release must include the following features:

### Core Functionality
- Query parameter endpoint functional (`?format=markdown`)
- REST API endpoint functional (`/wp-json/wp-to-markdown/v1/content/`)
- Basic HTML-to-Markdown conversion (headings, paragraphs, lists, links, bold, italic, code)
- YAML front-matter generation with required fields
- Support for Posts and Pages

### Admin Interface
- Settings page under Settings menu
- Global enable/disable toggle
- Post type selection
- Basic exclusion options (Post IDs)
- Public/Restricted access toggle
- Single API key generation

### Performance
- Transient-based caching
- Automatic cache invalidation on post update
- Basic rate limiting (60 requests/minute)

### Security
- Bearer token authentication option
- Proper HTTP status codes for all scenarios
- Input sanitization
- Capability checks for admin actions

### Compatibility
- WordPress 6.0+ compatibility
- PHP 8.1+ compatibility
- Gutenberg block support

### Documentation
- readme.txt for WordPress.org
- Basic README.md
- Inline code documentation

## 13.2 Version 1.0 Release Criteria

All MVP criteria plus:

### Enhanced Functionality
- Index/discovery endpoint
- Search endpoint
- Image handling options
- Embed conversion
- Custom field support
- Taxonomy inclusion in front-matter

### Page Builder Support
- Elementor content extraction
- Divi content extraction
- WPBakery content extraction
- Beaver Builder content extraction
- Oxygen Builder content extraction
- Bricks Builder content extraction

### Admin Interface
- Tabbed settings interface
- Multiple API key management
- URL pattern exclusions
- CSS class exclusions
- Cache TTL configuration
- Request logging and basic analytics
- Dashboard widget

### Discovery Protocol
- robots.txt AI-Markdown directives
- Well-known JSON endpoint
- HTTP header discovery
- HTML meta tag discovery
- llms.txt support

### Performance
- Object cache support
- Warm cache on publish option
- Conditional GET support (ETag, Last-Modified)

### Security
- IP whitelist/blacklist
- User-Agent filtering
- Configurable rate limiting

### Compatibility
- Elementor verified
- Yoast SEO verified
- ACF verified
- WooCommerce basic support

### Documentation
- Full API documentation
- Hooks reference
- Configuration guide
- FAQ
- Troubleshooting guide

## 13.3 Definition of Done

A feature is considered complete when:

- Code is written and follows WordPress coding standards
- Unit tests written and passing
- Integration tests written and passing
- Code reviewed and approved by at least one other developer
- Documentation updated (user and/or developer)
- Changelog entry added
- No PHP warnings or errors
- PHPCS passes without errors
- PHPStan passes at level 6
- Manual QA completed
- Accessibility reviewed (for admin UI changes)

---

# 14. Future Roadmap

## 14.1 Phase 1: Foundation (v1.0 - v1.1)

| Version | Planned Features |
|---------|------------------|
| 1.0 | MVP + full feature set, page builder support, discovery protocol |
| 1.0.1 | Bug fixes, performance improvements |
| 1.1 | WooCommerce product support, custom field mapping UI, advanced table handling |

## 14.2 Phase 2: Integration (v1.2 - v1.3)

| Version | Planned Features |
|---------|------------------|
| 1.2 | Webhook notifications on content changes, RSS/Atom alternative endpoint |
| 1.3 | GraphQL endpoint option, Zapier integration, advanced analytics dashboard |

## 14.3 Phase 3: AI Optimization (v2.0 - v2.1)

| Version | Planned Features |
|---------|------------------|
| 2.0 | Token counting display, content chunking for context windows, summary generation |
| 2.1 | Full llms.txt standard support, AI-specific metadata, relevance scoring |

## 14.4 Phase 4: Enterprise (v3.0+)

| Version | Planned Features |
|---------|------------------|
| 3.0 | Multi-site network support, role-based access control, audit logging |
| 3.1 | SaaS integration (managed API keys), usage-based billing support |
| 3.2 | Custom endpoint builder, visual configuration UI |

## 14.5 Potential Future Features (Backlog)

- Real-time content synchronization
- Bi-directional sync (Markdown to WordPress)
- AI training data export (JSONL format)
- Content versioning API
- Diff endpoint (what changed)
- Bulk download (ZIP archive)
- CLI commands (WP-CLI integration)
- Block-level exclusions (Gutenberg)
- Scheduled content unlocking
- Geographic access restrictions
- A/B testing for AI content variants
- Custom page builder support framework
- AI-powered content summarization
- Multi-language content translation API

---

# 15. Appendix

## 15.1 Sample Markdown Output

```markdown
---
title: "Complete Guide to WordPress Development"
description: "Learn everything about WordPress development, from themes to plugins."
date_published: "2024-06-15T14:30:00Z"
date_modified: "2025-01-10T09:15:00Z"
author: "Jane Developer"
permalink: "https://example.com/wordpress-development-guide/"
slug: "wordpress-development-guide"
type: "post"
status: "published"
featured_image: "https://example.com/wp-content/uploads/wp-dev-guide.jpg"
categories:
  - "Development"
  - "Tutorials"
tags:
  - "WordPress"
  - "PHP"
  - "Web Development"
language: "en-US"
word_count: 2450
reading_time: "10 min"
builder: "elementor"
---

# Complete Guide to WordPress Development

WordPress powers over 40% of the web, making it the most popular content management system in the world. In this comprehensive guide, we'll explore everything you need to know to become a proficient WordPress developer.

## Getting Started

Before diving into WordPress development, ensure you have the following prerequisites:

- **PHP 8.1+** - WordPress is built on PHP
- **MySQL 5.7+** - For database management
- **A local development environment** - Such as Local by Flywheel or Docker

### Setting Up Your Environment

1. Download and install Local by Flywheel
2. Create a new WordPress site
3. Install your preferred code editor

> **Pro Tip:** Use VS Code with the PHP Intelephense extension for the best development experience.

## Understanding WordPress Architecture

WordPress follows a modular architecture consisting of:

| Component | Description |
|-----------|-------------|
| Core | The main WordPress software |
| Themes | Control the visual presentation |
| Plugins | Extend functionality |
| Database | Stores all content and settings |

## Conclusion

WordPress development offers endless possibilities.

![WordPress Logo](https://example.com/wp-content/uploads/wordpress-logo.png "The WordPress Logo")
```

## 15.2 Sample API Responses

### 15.2.1 Index Endpoint Response

```json
{
  "total": 156,
  "page": 1,
  "per_page": 20,
  "total_pages": 8,
  "items": [
    {
      "id": 42,
      "title": "Complete Guide to WordPress Development",
      "url": "https://example.com/wordpress-development-guide/",
      "markdown_url": "https://example.com/wordpress-development-guide/?format=markdown",
      "type": "post",
      "date_modified": "2025-01-10T09:15:00Z",
      "author": "Jane Developer"
    }
  ],
  "_links": {
    "self": "https://example.com/wp-json/wp-to-markdown/v1/index/?page=1",
    "next": "https://example.com/wp-json/wp-to-markdown/v1/index/?page=2"
  }
}
```

### 15.2.2 Status Endpoint Response

```json
{
  "status": "ok",
  "version": "1.0.0",
  "wordpress_version": "6.4.2",
  "php_version": "8.2.0",
  "endpoints": {
    "content": true,
    "index": true,
    "search": true
  },
  "cache": {
    "enabled": true,
    "type": "object_cache",
    "hit_rate": 0.94
  },
  "total_content_items": 156,
  "timestamp": "2025-01-15T10:30:00Z"
}
```

## 15.3 Glossary

| Term | Definition |
|------|------------|
| AI Agent | Automated system that browses and processes web content |
| Bearer Token | Authentication credential passed in the Authorization header |
| CDN | Content Delivery Network - distributed caching infrastructure |
| ETag | Entity tag for HTTP cache validation |
| Front-matter | YAML metadata block at the beginning of a Markdown document |
| GPTBot | OpenAI's web crawler User-Agent identifier |
| LLM | Large Language Model - AI systems like GPT, Claude, etc. |
| Markdown | Lightweight markup language for formatted text |
| Object Cache | In-memory caching system (Redis, Memcached) |
| Rate Limiting | Restricting request frequency to prevent abuse |
| REST API | Representational State Transfer Application Programming Interface |
| Transient | WordPress temporary data storage mechanism |
| YAML | YAML Ain't Markup Language - human-readable data serialization |

## 15.4 Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 0.1 | 2025-01-10 | Product Development Team | Initial draft |
| 0.2 | 2025-01-15 | Product Development Team | Added missing sections, page builder support |
| 1.0 | TBD | Product Development Team | Final approved specification |

---

**End of Document**

This specification document defines the complete requirements for the WP-to-Markdown API for AI Agents WordPress plugin. For implementation details, code samples, and technical documentation, please refer to the companion developer documentation.
