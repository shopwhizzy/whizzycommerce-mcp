# WhizzyCommerce MCP

Run your online shop from Claude: orders, prices, sales, catalogue, translations and design, with nothing changing until you confirm.

[WhizzyCommerce](https://whizzycommerce.com/?ref=mcpmarket) is an ecommerce platform that is free until you sell. Its remote MCP server lets Claude, ChatGPT or any MCP client read the shop and propose changes in plain language.

This repository holds the documentation for the connector and its plugin manifests for Claude and ChatGPT. The server is hosted by WhizzyCommerce; there is nothing to run.

## How it works

- **Reads are direct.** Orders, products, customers, sales, stock, settings.
- **Writes are proposals.** A tool that would change the shop returns a preview of every row before and after, plus a confirmation link. Only the merchant, signed in to the WhizzyCommerce dashboard, can apply it.
- **Every change leaves a receipt.** It lands in the shop's change log with who proposed it and who confirmed it, and most changes can be undone from there. An email that has been sent, or money that has left, cannot.
- **Design edits are drafts.** Theme changes never touch the live shop until the merchant publishes them.
- **Scoped access.** Out of the box a connection can only propose price changes and sales. The merchant widens that, or picks a named agent: Analyst (reports only), Shopkeeper (orders, catalogue, reports), Merchandiser (prices, catalogue, reports) or Front desk (customers, orders, reports). None of them includes refunds.

## Connect

| | |
|---|---|
| Endpoint | `https://app.whizzycommerce.com/mcp` |
| Transport | Streamable HTTP |
| Auth | OAuth 2.1 with PKCE and dynamic client registration |

One address for every shop: when you sign in, you pick the shop the connection is for. Each shop also has its own address, `https://app.whizzycommerce.com/mcp/<your-shop-id>`, on the shop's **AI** screen in the dashboard.

### claude.ai

1. Settings, Connectors, **Add custom connector**.
2. Paste the address and name it `whizzy-merchant`.
3. Sign in to WhizzyCommerce, pick the shop and allow the connection.

Custom connectors need a Claude plan that allows them; that plan pays for the thinking.

### Claude Code

```sh
claude mcp add --transport http whizzy-merchant https://app.whizzycommerce.com/mcp
claude
/mcp     # whizzy-merchant, then Authenticate
```

### ChatGPT

Settings, Apps, Advanced settings. Turn on Developer mode, then Create and paste the address.

### As a plugin

This repository is a plugin for both Claude Code and ChatGPT: `.claude-plugin/plugin.json` with `.mcp.json` for Claude, `plugin.json` with `mcp.json` for ChatGPT and Codex.

```sh
claude plugin marketplace add shopwhizzy/whizzycommerce-mcp
claude plugin install whizzycommerce@whizzycommerce
```

### Other clients (`.mcp.json`)

```json
{
  "mcpServers": {
    "whizzy-merchant": {
      "type": "http",
      "url": "https://app.whizzycommerce.com/mcp"
    }
  }
}
```

## Use cases

1. **Morning order check.** "Which paid orders haven't gone out yet?" lists paid orders not fully shipped, with number, date and total.
2. **Shipping orders.** "Order 1042 went with CTT today, tracking RR123456789PT. Mark it sent." Previews the status change and the dispatch email with the tracking link.
3. **Customer service replies.** "Ana is asking where order 1038 is. Check it and write her a short reply." Looks up the order and drafts the email for you to confirm.
4. **Price changes.** "Raise every price in the Autumn collection by 5%." One preview with every variant, old and new price. By collection, tag, vendor or list, up to 2,000 variants at a time.
5. **Sales and discount codes.** "Make AUTUMN10: 10% off orders over 40 euros, once per customer, until the end of November."
6. **Product copy.** "Write a description for the new speckled mug: 350 ml, stoneware, dishwasher safe." Old and new text side by side.
7. **Translating the shop.** "Add French and Spanish, then translate everything still missing." Finds untranslated products, collections, pages and menus, and proposes translations in batches of up to 200 fields.
8. **Search titles and descriptions.** "Give the ten best sellers better search titles and descriptions."
9. **Sales review.** "October felt slow. Compare it with September." Reads daily sales, best sellers, low stock and running promotions. It sees orders, not visitors.
10. **Weekly brief.** "Every Monday at 8, send me a summary and anything that needs doing." A routine that emails you; suggested changes arrive as confirm links.
11. **Catalogue imports.** Reads a supplier's CSV or XML file before importing, then sets up or runs the import.
12. **Shop setup.** Delivery methods and areas, tax rates, currencies, policy pages, logo and name, and shopping feeds for Google Merchant Center and Meta.
13. **Theme design.** "Make the header darker and add a banner for the autumn sale." Edits a draft you publish from Design.
14. **Handing the shop to a helper.** A scoped agent such as Front desk or Merchandiser instead of full access.

## What it can do

- **Orders:** look up, notes, create, mark sent or delivered, correct tracking, email payment links
- **Catalogue:** products, categories, pictures, publishing, CSV and XML imports
- **Pricing:** bulk price changes, sales, discount codes, gift cards
- **Customers:** look up, add, notes, email, sign-in links
- **Languages:** shop languages, untranslated content, stored translations
- **Search and feeds:** search titles and descriptions, redirects, shopping feeds
- **Settings:** delivery, collection points, tax, currencies, policies, plugins, reviews, opening or closing the shop
- **Routines:** scheduled briefs and checks
- **Design:** theme edits as drafts

## FAQ

**Can Claude change my shop without me?** No. Changing tools only return a preview and a link; the change happens when you confirm in the dashboard.

**What does it cost?** WhizzyCommerce Free has no monthly fee and charges 1.5% per sale. The AI runs on your own Claude plan.

**Can it issue refunds?** No starting role includes refunds.

## Links

- Website: https://whizzycommerce.com
- Connecting Claude: https://whizzycommerce.com/developers/claude

## License

MIT for the contents of this repository. The WhizzyCommerce service has its own terms.
