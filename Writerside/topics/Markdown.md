# Markdown

I wrote markdown files in a GitHub repo to build documentation websites for both developers and end users. This page illustrates semantic markup techniques, like conditionally rendered text and global variables, for the end-user documentation. 

## The challenge
Intellicheck Hub is the company's flagship SaaS solution for identity validation. Hub includes three components: backend administration, an associate portal, and a standalone Windows desktop app. These three components are named Hub, Portal, and Desktop.

The previous-generation documentation had been written in Microsoft Word and delivered to customers with Adobe PDF files. Corporate leadership desperately wanted to move away from PDF-based delivery to a modern online experience. I evaluated authoring tools that could scale on a budget and landed on JetBrains Writerside. Unbeknownst to me, our eight-person dev team already used JetBrains IDEs. That overlap (and the fact that Writerside is a free plugin) made adoption an easy sell.

I built several documentation websites with a single docs-as-code workflow and automated GitHub actions. The sites included context-sensitive links from software interface pages to the corresponding documentation site pages. I wrote task-based procedures, reference material, common troubleshooting steps, FAQs, and release notes.

## Skills demonstrated

- **Structured authoring with reusable content** in a shared snippet library for warnings, tips, and role requirements, referenced across dozens of topics instead of copy-pasted.
- **Conditional text and variables** to manage product-specific content from a single-source repository that documented all software offerings, (Hub, Portal and Desktop).
- **Procedural writing** using Writerside's step, tab, and tooltip elements for consistent, testable instructions.
- **A standardized release notes template** with defined sections (features, improvements, bug fixes by product area) and Jira issue references.
- **GitHub Actions-driven publishing** to separate internal and external environments, with a change-request requirement for production releases.

## Mixed markdown and semantic markup

I leveraged semantic markup in the standard markdown (.md) files. This provided for a richer design palette when necessary without the constraints of structured XML (.topic) files. The following table describes various markdown and semantic markup elements that appear in the files. 

<table width="400">
  <tr>
    <td><control>Element</control></td>
    <td><control>Type</control></td>
    <td><control>Description</control></td>
    <td><control>Example</control></td>
  </tr>
  <tr>
    <td>Headings</td>
    <td>Markdown</td>
    <td>Headings are prefixed with the pound symbol.</td>
    <td><code># Log In</code></td>
  </tr>
  <tr>
    <td>Links</td>
    <td>Markdown</td>
    <td>Links use standard markdown notation with square brackets and parenthesis.</td>
    <td><code>[https://hub.intellicheck.com](https://hub.intellicheck.com)</code></td>
  </tr>
  <tr>
    <td>Conditionally tagged content</td>
    <td>Semantic markup</td>
    <td>Conditional content is wrapped in statements that render procedure steps for either Hub or Desktop. Steps that only apply to Hub software do not appear in the Desktop content, and vice versa.</td>
    <td><code>&lt;if instance="h"&gt;</code></td>
  </tr>
  <tr>
    <td>Admonitions</td>
    <td>Markdown</td>
    <td>Callouts use the greater-than symbol.</td>
    <td><code>&gt; Register</code></td>
  </tr>
  <tr>
    <td>Procedures</td>
    <td>Semantic markup</td>
    <td>A procedure includes a title and numbered steps.</td>
    <td><code>&lt;procedure&gt;&lt;title&gt;Log in procedure&lt;/title&gt;</code></td>
  </tr>
  <tr>
    <td>Numbered steps</td>
    <td>Semantic markup</td>
    <td>Steps are auto-numbered within their parent procedure.</td>
    <td><code>&lt;step&gt; Type an **Email**, and then click **Login**.&lt;/step&gt;</code></td>
  </tr>
  <tr>
    <td>Variables</td>
    <td>Semantic markup</td>
    <td>Variables can be used for various reasons, for example, to future-proof software product names that might change due to rebranding.</td>
    <td><code>%​product-desktop%</code></td>
  </tr>
  <tr>
    <td>Tabs</td>
    <td>Semantic markup</td>
    <td>Tabs create divided boxes for related content such as code samples or images.</td>
    <td><code>&lt;tabs&gt;&lt;tab title="Email"&gt;</code></td>
  </tr>
  <tr>
    <td>Images</td>
    <td>Markdown</td>
    <td>Image syntax looks like markdown link syntax, but prefixed with an exclamation mark.</td>
    <td><code>![Type password](login-hub-password.svg)</code></td>
  </tr>
  <tr>
    <td>Tables</td>
    <td>Markdown</td>
    <td>Pipes and dashes create markdown tables.</td>
    <td><code>|-------</code></td>
  </tr>
</table>

## Example markdown file
Below is the raw text from a markdown file that describes a simple login procedure. It demonstrates how one topic file can include both standard markdown and semantic markup to maintain a single source for two software products (Hub and Desktop). 

```markdown
# Log In

<if instance="h">

Log in here: [https://example.login.url](https://www​.example.login.url)

</if>

> **Register**
> 
> To log in, you must be a registered customer.
> - Contact [Example support link](https://www%​.example.com/support).
>
{style="note"}

![Login flow](login-flow-hub.svg){thumbnail="true" width="800"}

<procedure><title>Log in procedure</title>

To log in, follow these steps:

<if instance="d">

<step>

Start %​product-desktop%
</step>

</if>

<step>

Type an **Email** and **Password**, and then click **Login**.
</step>

<step>

Select a two-factor authentication (2FA) method.

* Click **Via SMS** to receive a text.
* Click **Via Email** to receive an email.
</step>

<step>

Type the verification code, and then click **Verify**.
</step>

Log in complete.



<tabs>
<tab title="Email and Password">

![Type email and password](login-email-password.svg)
</tab>

<tab title="2FA">

![Select 2FA](login-2fa.svg)
</tab>

<tab title="Verify Code">

![Type code](login-verify.svg)
</tab>
</tabs>


</procedure>



## Log in error messages


| Error                                                                 | Resolution                                                                                                                                                                                                                                                                                  |
|-----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wrong credentials. Try again.                                         | Reenter your email and password. After six failed attempts, the account locks.                                                                                                                                                                                    |
| Login attempts exceeded. Wait 24 hours or contact your administrator. | Your account is locked. Contact your admin or [Example support link](https://wwww.example.com/support).                                                                                                                                                |
| Inactive account. Check email for activation instructions.            | Ask an admin to confirm your account and customer status are both active. Inactive users can't log in. For help, contact [Example support link](https://www.example.com/support) for help. |
| Company locked. Wait 24 hours or contact your administrator.          | Your company configuration has a problem. Contact support.                                                                                                                                                                                                                   |
| Bad verification code                                                 | Retype the verification code.                                                                                                                                                                                                                                                               |
| Session not found.                                                    | Return to the login page and log in again.                                                                                                                                                                                                                                                    |



```



> **See it!**
> 
> Click below to see how the markdown renders into a page for a documentation website.
> 
> --> [Click here to see the Log In help page](LogIn.md) <--
> 
{style="tip"}