# Curl Calls

## Settings

- MCP endpoint: `https://carts.anigok.com/mcp`
- Method: `POST`
- Header: `Content-Type: application/json`
- Header: `Accept: application/json, text/event-stream`
- Transport requirement: the current MCP handler requires both `application/json` and `text/event-stream` in the `Accept` header

## hello

```bash
curl -X POST https://carts.anigok.com/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "tools/call",
    "params": {
      "name": "hello",
      "arguments": {
        "name": "World"
      }
    }
  }'
```

## create_cart

```bash
curl -X POST https://carts.anigok.com/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "tools/call",
    "params": {
      "name": "create_cart",
      "arguments": {
        "shop_domain": "your-shop-domain.myshopify.com",
        "meta": {
          "ucp-agent": {
            "profile": "https://example.com/.well-known/ucp"
          }
        },
        "cart": {
          "line_items": [
            {
              "quantity": 2,
              "item": {
                "id": "gid://shopify/ProductVariant/12345678901"
              }
            }
          ],
          "context": {
            "address_country": "US",
            "address_region": "CA",
            "postal_code": "94105"
          },
          "attribution": {
            "referring_domain": "example-agent.com",
            "click_id_tag": "gclid",
            "click_id_value": "abc123xyz",
            "activity_id_tag": "activity_id",
            "activity_id_value": "cart-start-001",
            "utm_campaign": "spring_sale",
            "utm_source": "example_agent",
            "utm_medium": "agentic_commerce",
            "utm_content": "sweater_recommendation",
            "utm_term": "organic cotton sweater"
          }
        }
      }
    }
  }'
```

## get_cart

```bash
curl -X POST https://carts.anigok.com/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "tools/call",
    "params": {
      "name": "get_cart",
      "arguments": {
        "shop_domain": "your-shop-domain.myshopify.com",
        "meta": {
          "ucp-agent": {
            "profile": "https://example.com/.well-known/ucp"
          }
        },
        "id": "gid://shopify/Cart/cart_abc123"
      }
    }
  }'
```

## update_cart

```bash
curl -X POST https://carts.anigok.com/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "tools/call",
    "params": {
      "name": "update_cart",
      "arguments": {
        "shop_domain": "your-shop-domain.myshopify.com",
        "meta": {
          "ucp-agent": {
            "profile": "https://example.com/.well-known/ucp"
          }
        },
        "id": "gid://shopify/Cart/cart_abc123",
        "cart": {
          "line_items": [
            {
              "quantity": 3,
              "item": {
                "id": "gid://shopify/ProductVariant/12345678901"
              }
            },
            {
              "quantity": 1,
              "item": {
                "id": "gid://shopify/ProductVariant/22222222222"
              }
            }
          ],
          "context": {
            "address_country": "US",
            "address_region": "CA",
            "postal_code": "94105"
          },
          "attribution": {
            "utm_source": "example_agent",
            "utm_medium": "agentic_commerce",
            "utm_campaign": "spring_sale"
          }
        }
      }
    }
  }'
```

## cancel_cart

```bash
curl -X POST https://carts.anigok.com/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "tools/call",
    "params": {
      "name": "cancel_cart",
      "arguments": {
        "shop_domain": "your-shop-domain.myshopify.com",
        "meta": {
          "ucp-agent": {
            "profile": "https://example.com/.well-known/ucp"
          },
          "idempotency-key": "660e8400-e29b-41d4-a716-446655440001"
        },
        "id": "gid://shopify/Cart/cart_abc123"
      }
    }
  }'
```

## Notes

- The caller provides `shop_domain`.
- The Worker maps `shop_domain` to `https://{shop-domain}/api/ucp/mcp`.
- The caller provides `meta["ucp-agent"].profile`.
- `cancel_cart` requires `meta["idempotency-key"]` as a UUID.
- `update_cart` sends the full cart state. Omitted fields are removed.
- Calls to this endpoint must include `Accept: application/json, text/event-stream`.
