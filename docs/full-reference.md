# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [shipping_api/fulfillment/service.proto](#shipping_api_fulfillment_service-proto)
    - [AcknowledgeFulfillmentOrdersRequest](#shipping_api-AcknowledgeFulfillmentOrdersRequest)
    - [AcknowledgeFulfillmentOrdersResponse](#shipping_api-AcknowledgeFulfillmentOrdersResponse)
    - [Acknowledgement](#shipping_api-Acknowledgement)
    - [AcknowledgementResult](#shipping_api-AcknowledgementResult)
    - [Address](#shipping_api-Address)
    - [Alert](#shipping_api-Alert)
    - [AlertResolution](#shipping_api-AlertResolution)
    - [AlertResolveResult](#shipping_api-AlertResolveResult)
    - [AlertResult](#shipping_api-AlertResult)
    - [CancellationConfirmation](#shipping_api-CancellationConfirmation)
    - [CancellationLine](#shipping_api-CancellationLine)
    - [CancellationRequest](#shipping_api-CancellationRequest)
    - [CancellationResult](#shipping_api-CancellationResult)
    - [Check](#shipping_api-Check)
    - [ConfirmCancellationsRequest](#shipping_api-ConfirmCancellationsRequest)
    - [ConfirmCancellationsResponse](#shipping_api-ConfirmCancellationsResponse)
    - [ConfirmShipmentsRequest](#shipping_api-ConfirmShipmentsRequest)
    - [ConfirmShipmentsResponse](#shipping_api-ConfirmShipmentsResponse)
    - [FulfillmentOrder](#shipping_api-FulfillmentOrder)
    - [FulfillmentOrderLine](#shipping_api-FulfillmentOrderLine)
    - [GetFulfillmentOrderRequest](#shipping_api-GetFulfillmentOrderRequest)
    - [IntegrationStatusRequest](#shipping_api-IntegrationStatusRequest)
    - [IntegrationStatusResponse](#shipping_api-IntegrationStatusResponse)
    - [ListNeedToCancelRequest](#shipping_api-ListNeedToCancelRequest)
    - [ListNeedToCancelResponse](#shipping_api-ListNeedToCancelResponse)
    - [ListNeedToShipRequest](#shipping_api-ListNeedToShipRequest)
    - [ListNeedToShipResponse](#shipping_api-ListNeedToShipResponse)
    - [Money](#shipping_api-Money)
    - [RaiseAlertsRequest](#shipping_api-RaiseAlertsRequest)
    - [RaiseAlertsResponse](#shipping_api-RaiseAlertsResponse)
    - [RejectFulfillmentOrdersRequest](#shipping_api-RejectFulfillmentOrdersRequest)
    - [RejectFulfillmentOrdersResponse](#shipping_api-RejectFulfillmentOrdersResponse)
    - [Rejection](#shipping_api-Rejection)
    - [RejectionResult](#shipping_api-RejectionResult)
    - [ResolveAlertsRequest](#shipping_api-ResolveAlertsRequest)
    - [ResolveAlertsResponse](#shipping_api-ResolveAlertsResponse)
    - [Shipment](#shipping_api-Shipment)
    - [ShipmentLine](#shipping_api-ShipmentLine)
    - [ShipmentResult](#shipping_api-ShipmentResult)
    - [WarehouseStatusRequest](#shipping_api-WarehouseStatusRequest)
    - [WarehouseStatusResponse](#shipping_api-WarehouseStatusResponse)
  
    - [AlertType](#shipping_api-AlertType)
    - [CancellationReason](#shipping_api-CancellationReason)
    - [CheckSource](#shipping_api-CheckSource)
    - [CheckState](#shipping_api-CheckState)
    - [FulfillmentOrderStatus](#shipping_api-FulfillmentOrderStatus)
    - [RejectionReason](#shipping_api-RejectionReason)
  
    - [FulfillmentIntegrationService](#shipping_api-FulfillmentIntegrationService)
    - [WarehouseService](#shipping_api-WarehouseService)
  
- [Scalar Value Types](#scalar-value-types)



<a name="shipping_api_fulfillment_service-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## shipping_api/fulfillment/service.proto



<a name="shipping_api-AcknowledgeFulfillmentOrdersRequest"></a>

### AcknowledgeFulfillmentOrdersRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| acknowledgements | [Acknowledgement](#shipping_api-Acknowledgement) | repeated | One entry per fulfillment order you have created on your own side. Required and server-capped: an empty list, or one over the cap, fails the whole request rather than answering per entry. |






<a name="shipping_api-AcknowledgeFulfillmentOrdersResponse"></a>

### AcknowledgeFulfillmentOrdersResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [AcknowledgementResult](#shipping_api-AcknowledgementResult) | repeated | One result per acknowledgement sent. Match on fulfillment_order_id rather than on position, and read every entry: one failing does not fail the rest. |






<a name="shipping_api-Acknowledgement"></a>

### Acknowledgement



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | The work you are claiming, as ListNeedToShip gave it. Acknowledging does not drain it from that queue — you still owe the shipment. |
| external_order_id | [string](#string) |  | Your own identifier for the work. Unique per integration — Zentail rejects a duplicate rather than recording it twice, which is what makes a retry safe. |






<a name="shipping_api-AcknowledgementResult"></a>

### AcknowledgementResult



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | The acknowledgement this answers, echoed back. |
| success | [bool](#bool) |  | True when Zentail holds your external_order_id against this fulfillment order — including when it already did, which reports success with already_acknowledged set. False is always a real failure. |
| error_message | [string](#string) |  | Why it failed, in prose, for logs and support. Empty on success. This contract carries no error code, so the string is all there is: log it, and do not branch on its wording. |
| already_acknowledged | [bool](#bool) |  | True when this external_order_id was already recorded — a replay, not a conflict. Treat as success. |






<a name="shipping_api-Address"></a>

### Address
Address is the buyer&#39;s shipping address exactly as the sales channel supplied
it. Zentail copies it through without normalising, validating or completing
any part of it, so treat every field as free text — in particular country and
region are **not** guaranteed to be ISO codes.

Validate it against your own carrier&#39;s rules and reject with
UNDELIVERABLE_ADDRESS rather than shipping to a guess. An address that was
never captured, or that has been redacted under the PII retention policy, is
absent from the fulfillment order entirely rather than arriving half-filled.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  | Who the package is addressed to. Not always the buyer — see FulfillmentOrder.buyer_name. |
| company | [string](#string) |  | Business name at the delivery address, when the channel captured one. |
| line_1 | [string](#string) |  | Street address. The line that is present whenever there is an address at all. |
| line_2 | [string](#string) |  | Apartment, suite, unit or similar. Commonly empty. |
| city | [string](#string) |  | Town or city, as supplied. Free text, and empty when the channel sent none. |
| region | [string](#string) |  | State, province or county, as supplied. Sometimes a code and sometimes a full name, because it is whatever the channel sent — do not key on it. |
| postal_code | [string](#string) |  | ZIP or postal code, as supplied. Not validated, and not checked against the country. |
| country | [string](#string) |  | Country, as supplied. Usually a two-letter ISO code but not guaranteed to be one, so anything parsing it needs a fallback. |
| phone | [string](#string) |  | Contact number for the carrier, when the channel captured one. Not normalised to E.164 and may carry extensions or punctuation. |
| email | [string](#string) |  | The buyer&#39;s email address, for carrier notifications about this delivery. It hangs off the address rather than the order because the two are PII under one retention policy: a redacted order carries neither. |






<a name="shipping_api-Alert"></a>

### Alert



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | The fulfillment order the problem is on. An alert is always about one piece of work; there is no integration-wide alert — IntegrationStatus is where that belongs. |
| type | [AlertType](#shipping_api-AlertType) |  | What kind of problem it is. Required, and it is half the identity of the alert: Zentail refuses a second open alert of the same type on the same order, so the type is what makes a poller&#39;s re-raise a no-op. |
| message | [string](#string) |  | Shown to the user. Say what is wrong and what would fix it. |
| line_item_id | [string](#string) |  | Optional: scope the alert to one line. |






<a name="shipping_api-AlertResolution"></a>

### AlertResolution



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | The fulfillment order carrying the alert. |
| type | [AlertType](#shipping_api-AlertType) |  | Which alert to clear. Only an alert this integration raised is in scope, so an alert of the same type raised by Zentail or by another integration survives. |
| resolution | [string](#string) |  | Why it is resolved. Stored alongside the alert and shown to the user. |






<a name="shipping_api-AlertResolveResult"></a>

### AlertResolveResult
AlertResolveResult mirrors AlertResult but carries the resolve path&#39;s own
soft-success flag. AlertResult&#39;s already_open only makes sense when raising;
on a retried resolve it would be nonsense, leaving a caller that timed out
and retried with no honest way to read success.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | The fulfillment order the alert was on, echoed back. |
| type | [AlertType](#shipping_api-AlertType) |  | The alert type, echoed back. Match a result on this and fulfillment_order_id together. |
| success | [bool](#bool) |  | True when no alert of this type is open any more, whether this call closed it or found nothing to close. False is a real failure. |
| error_message | [string](#string) |  | Why it failed, in prose, for logs and support. Empty on success. |
| already_resolved | [bool](#bool) |  | True when no open alert of this type remained, so nothing changed. Treat as success: it is what a retry after a timeout sees, and what a poller sees when a user resolved the alert by hand first. |






<a name="shipping_api-AlertResult"></a>

### AlertResult



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | The fulfillment order the alert was raised against, echoed back. |
| type | [AlertType](#shipping_api-AlertType) |  | The alert type, echoed back. Match a result on the pair of this and fulfillment_order_id — together they identify the alert. |
| success | [bool](#bool) |  | True when an open alert of this type exists on the order, whether this call created it or found it already there. False is a real failure. |
| error_message | [string](#string) |  | Why it failed, in prose, for logs and support. Empty on success. A missing `message` and a type an integration may not raise both land here. |
| already_open | [bool](#bool) |  | True when an open alert of this type already existed, so nothing was created. Treat as success — this is the expected steady state for a poller re-raising a condition that has not gone away. |






<a name="shipping_api-CancellationConfirmation"></a>

### CancellationConfirmation



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | Which cancellation request you are answering, from ListNeedToCancel. |
| lines | [ShipmentLine](#shipping_api-ShipmentLine) | repeated | Not supported yet. Sending any line fails the entry rather than being widened to the whole request, because treating a partial confirmation as a full one would cancel units you did not. Omit it to confirm the whole request, which is the only shape ListNeedToCancel asks for today. |
| already_shipped | [bool](#bool) |  | Set when you could not cancel because the units are already on their way. Zentail keeps the customer order truthful rather than showing a cancellation that did not happen; report the shipment through ConfirmShipments as normal. |






<a name="shipping_api-CancellationLine"></a>

### CancellationLine
CancellationLine is deliberately not a FulfillmentOrderLine. On a
fulfillment order, quantity means &#34;routed to your warehouse&#34;; on a cancel it
means &#34;pull this many back&#34;, and shipped_quantity / cancelled_quantity have
no meaning on an instruction at all. An integration that reuses one line
parser across both would read the wrong number.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| line_item_id | [string](#string) |  | Echoes the fulfillment order&#39;s line, so a partial cancel names exactly which line it reduces. |
| sku | [string](#string) |  | The SKU on that line, so a warehouse operator can read the instruction. line_item_id is what identifies the line — one SKU can appear on two. |
| quantity | [int32](#int32) |  | How many units to pull back — not the line&#39;s routed total. |






<a name="shipping_api-CancellationRequest"></a>

### CancellationRequest
CancellationRequest is Zentail asking for work back.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | The work being pulled back, the same id ListNeedToShip gave you. This is the only identifier ConfirmCancellations accepts. |
| external_order_id | [string](#string) |  | Your identifier, from the acknowledgement — the handle you need to find it on your own side. |
| order_number | [string](#string) |  | The customer order this came from, for display and correlation. Not an identifier: one customer order can produce two fulfillment orders for you. |
| warehouse_unique_id | [string](#string) |  | The warehouse the work was routed to, in your own namespace. Carried so a multi-warehouse integration can route the pull-back without re-reading the fulfillment order. |
| reason | [CancellationReason](#shipping_api-CancellationReason) |  | Why, so it can be shown to a warehouse operator. |
| lines | [CancellationLine](#shipping_api-CancellationLine) | repeated | The quantities to pull back. A partial cancellation lists only some lines, or a lower quantity than the fulfillment order carries — ship the rest. |
| requested_ts | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  | When Zentail asked for the work back. For ageing a queue you have not drained — it is not a deadline, and not an idempotency key. |






<a name="shipping_api-CancellationResult"></a>

### CancellationResult



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | The confirmation this answers, echoed back. |
| success | [bool](#bool) |  | True when the request is drained, which includes a replay (already_recorded set) and a report of already_shipped — you answered the question you were asked. False is a real failure, and the request will be re-offered. |
| error_message | [string](#string) |  | Why it failed, in prose, for logs and support. Empty on success. Two failures are worth handling rather than retrying: no outstanding request for this order, and an order that has already shipped or been rejected. |
| already_recorded | [bool](#bool) |  | True when this cancellation was already recorded. Treat as success. |






<a name="shipping_api-Check"></a>

### Check



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  | Stable identifier, not prose — an operator or an alert matches on this, so it must not change when the wording does. Lower_snake_case by convention. |
| state | [CheckState](#shipping_api-CheckState) |  | How bad this is. FAIL means the thing the check covers is broken now; WARN means it still works but is degrading, which is where a queue that has stopped being drained shows up first. |
| message | [string](#string) |  | Prose for a human. Say what is wrong and what would fix it. |
| source | [CheckSource](#shipping_api-CheckSource) |  | Who observed this. Zentail sets it; an integration filling it in on a WarehouseStatus response has it overwritten with INTEGRATION. |
| warehouse_unique_id | [string](#string) |  | Which warehouse this check is about, when it is about one. Set by Zentail as it folds a WarehouseStatus response in, and on a check reporting that a warehouse could not be reached.

Without it the fold is lossy: two warehouses returning the same check name are indistinguishable, and anything matching on name alone cannot say which one is broken. Empty on checks about the integration as a whole. |






<a name="shipping_api-ConfirmCancellationsRequest"></a>

### ConfirmCancellationsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| confirmations | [CancellationConfirmation](#shipping_api-CancellationConfirmation) | repeated | One entry per cancellation request you have carried out. Required and server-capped, like every batch on this contract. |






<a name="shipping_api-ConfirmCancellationsResponse"></a>

### ConfirmCancellationsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [CancellationResult](#shipping_api-CancellationResult) | repeated | One result per confirmation sent, matched on fulfillment_order_id. An entry that failed is still in ListNeedToCancel on the next poll. |






<a name="shipping_api-ConfirmShipmentsRequest"></a>

### ConfirmShipmentsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| shipments | [Shipment](#shipping_api-Shipment) | repeated | One entry per package. Required and server-capped: an empty list, or one over the cap, fails the whole request rather than answering per entry. |






<a name="shipping_api-ConfirmShipmentsResponse"></a>

### ConfirmShipmentsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [ShipmentResult](#shipping_api-ShipmentResult) | repeated | One result per shipment sent. Match on external_shipment_id rather than on position, and read every entry — one package failing does not fail the others, and the units it covered are still owed. |






<a name="shipping_api-FulfillmentOrder"></a>

### FulfillmentOrder
FulfillmentOrder is a lean view of the work: only the lines routed to one of
the caller&#39;s warehouses, with the quantities routed there.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | Zentail&#39;s identifier for this unit of work, and the handle for every write in this API. Stable for the life of the fulfillment order. |
| order_number | [string](#string) |  | The customer order this came from. For display and correlation only — it is not unique to you, and is not accepted as an identifier on any write. |
| external_order_id | [string](#string) |  | Set once you have acknowledged it. |
| status | [FulfillmentOrderStatus](#shipping_api-FulfillmentOrderStatus) |  | Where the work has got to. Informational only, and possibly a poll behind the queues — see the enum for why nothing should branch on it. |
| warehouse_unique_id | [string](#string) |  | The warehouse these lines are routed to, in your own namespace. |
| ordered_ts | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  | When the buyer placed the customer order — not when the work reached you. Age your own queue on assigned_ts instead; an order can be routed to a warehouse days after it was placed. |
| assigned_ts | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  | When this work was routed to your warehouse. |
| last_updated_ts | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  | When Zentail last changed anything here, a re-route included. It tells you a cached copy is stale; it does not say what changed, so re-read `lines` rather than diffing on it. |
| ship_by_ts | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  | Deadline for handing the package to the carrier. Not populated: Zentail has no ship-by column to read, and will not approximate one from another timestamp because a fabricated deadline is worse than none to a fulfiller that prioritises on it. Always unset today — measured against sales-orders on 2026-09-21. Prioritise on assigned_ts. |
| ship_to | [Address](#shipping_api-Address) |  | Where the package goes. Unset when the order has no usable shipping address — none was captured, or it was redacted under the PII retention policy — and in that case there is nothing to ship to. Raise an alert or reject with UNDELIVERABLE_ADDRESS rather than guessing one. |
| requested_service_level | [string](#string) |  | The shipping speed the buyer bought, as Zentail&#39;s standardised service level for the order. Empty when the channel named none. Map it onto one of your own carrier services; it does not name a carrier. |
| buyer_name | [string](#string) |  | The buyer, for packing slips and support. Not the addressee: ship_to.name is who the label goes to and legitimately differs on a gift or a business delivery. Address the package from ship_to. |
| gift_message | [string](#string) |  | Not populated. Zentail has no gift-message column to read one from, so this is always empty and is not evidence that the order carries no gift message. Measured against sales-orders on 2026-09-21. |
| lines | [FulfillmentOrderLine](#shipping_api-FulfillmentOrderLine) | repeated | Authoritative on every read. Quantities change when Zentail re-routes work in or out of this warehouse, so never cache them across polls. |
| open_alerts | [Alert](#shipping_api-Alert) | repeated | Alerts you currently have open on this fulfillment order. Returned so a poller can see what it has already raised without keeping its own record — the same reason Zentail holds external_order_id. |
| shipping_price | [Money](#shipping_api-Money) |  | What the buyer paid to have the order shipped, and the tax on it. Order level, not per line: the buyer pays it once however the order splits, so a fulfillment order covering part of an order carries the whole figure and it must not be summed across siblings. |
| shipping_tax | [Money](#shipping_api-Money) |  | Tax charged on that shipping price. Order level on the same terms, so it must not be summed across the fulfillment orders of one customer order. |
| discount | [Money](#shipping_api-Money) |  | Discount applied to the order as a whole. |
| total_payment | [Money](#shipping_api-Money) |  | What the buyer actually paid, in total, on the channel they bought from.

Deliberately separate from the lines rather than derived from them. It is the transaction, and it can legitimately disagree with the composed total: an order part-refunded, cancelled, or not yet paid. Measured across ~37k real orders it matches the composition about 89% of the time, and the remainder is mostly lifecycle. per ZEN-4048.

So record payment from this, and compose the order from the lines. Do not reconcile one into the other by inventing an adjustment line. |






<a name="shipping_api-FulfillmentOrderLine"></a>

### FulfillmentOrderLine



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| line_item_id | [string](#string) |  | Stable per line; echo it back on shipment, cancellation and rejection. |
| sku | [string](#string) |  | Zentail&#39;s SKU for the unit to pick. Report work by line_item_id rather than by this: one SKU can appear on two lines of the same order. |
| title | [string](#string) |  | The product name, for packing slips and pick lists. Display only — it can change between polls and identifies nothing. |
| quantity | [int32](#int32) |  | Routed to your warehouse, not the customer order&#39;s total. |
| shipped_quantity | [int32](#int32) |  | Always zero, deliberately. `quantity` is already only what you still owe, so subtracting this would double-count what has shipped. Your own ConfirmShipments calls are the record of shipments, not this field. Measured against sales-orders on 2026-09-21. |
| cancelled_quantity | [int32](#int32) |  | Always zero, on the same terms as shipped_quantity. |
| unit_price | [Money](#shipping_api-Money) |  | What the buyer paid per unit, before tax. Not Zentail&#39;s cost.

Send this on to a store that prices the line itself, or it falls back to its own catalogue price and misstates the order, which is what happened before this field existed. per ZEN-4048. |
| tax | [Money](#shipping_api-Money) |  | Tax for the whole line, not per unit. Unset when the order is untaxed. |






<a name="shipping_api-GetFulfillmentOrderRequest"></a>

### GetFulfillmentOrderRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | Zentail&#39;s id, as it appears on any fulfillment order you have been given. |
| external_order_id | [string](#string) |  | The id you recorded with AcknowledgeFulfillmentOrders. Unique within your integration, so it resolves to exactly one fulfillment order — which is what makes it usable for reconciling after a crash. |






<a name="shipping_api-IntegrationStatusRequest"></a>

### IntegrationStatusRequest







<a name="shipping_api-IntegrationStatusResponse"></a>

### IntegrationStatusResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| checks | [Check](#shipping_api-Check) | repeated | Zentail&#39;s own observations and the integration&#39;s last reported checks, in one list. Read `source` to tell them apart; render them together, because an operator asking &#34;is this working&#34; does not care who noticed. |






<a name="shipping_api-ListNeedToCancelRequest"></a>

### ListNeedToCancelRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| cursor | [string](#string) |  | Leave empty for the first page; pass next_cursor thereafter. |
| page_size | [int32](#int32) |  | Server-capped. Omit for the default. |






<a name="shipping_api-ListNeedToCancelResponse"></a>

### ListNeedToCancelResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| cancellations | [CancellationRequest](#shipping_api-CancellationRequest) | repeated | What Zentail wants pulled back, one fulfillment order per entry. Only work you acknowledged appears here. Each entry is re-offered on every poll until you confirm it, so an entry you have already handled means the confirmation did not land. |
| next_cursor | [string](#string) |  | Empty when the page is the last one. |






<a name="shipping_api-ListNeedToShipRequest"></a>

### ListNeedToShipRequest
Paging for a queue. Cursor and page size only — a queue has no &#34;since&#34;,
because it is drained by confirming rather than by advancing a clock.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| cursor | [string](#string) |  | Leave empty for the first page; pass next_cursor thereafter. |
| page_size | [int32](#int32) |  | Server-capped. Omit for the default. |






<a name="shipping_api-ListNeedToShipResponse"></a>

### ListNeedToShipResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| orders | [FulfillmentOrder](#shipping_api-FulfillmentOrder) | repeated | The work you owe a shipment on, one fulfillment order per entry. Every read is authoritative rather than incremental: quantities on `lines` are what is routed to you now, and a re-route can lower them between polls. |
| next_cursor | [string](#string) |  | Empty when the page is the last one. |






<a name="shipping_api-Money"></a>

### Money
Money is an amount and the currency it is in.

The amount is a decimal string, not a float or units&#43;nanos. Money in this
contract crosses into Shopify&#39;s MoneyBagInput, which is itself a decimal
string, so a string maps across untouched — no scaling and no rounding at the
edge, which is where money bugs come from. Follows the Money already in
api-proto&#39;s listing contract rather than google.type.Money, for that reason.

currency_code travels with every amount rather than sitting once on the
order. Multi-currency is real (etp has
purchase_orders.non_standard_currencies), and a silently wrong currency is
worse than a wrong amount: the number still looks plausible.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| amount | [string](#string) |  | Decimal string, e.g. &#34;13.00&#34;. Negative for a credit. |
| currency_code | [string](#string) |  | ISO 4217, e.g. &#34;USD&#34;. |






<a name="shipping_api-RaiseAlertsRequest"></a>

### RaiseAlertsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| alerts | [Alert](#shipping_api-Alert) | repeated | One entry per condition to raise. Required and server-capped, like every batch on this contract. Safe to re-send the same alerts every poll: a second open alert of the same type on the same order is not created. |






<a name="shipping_api-RaiseAlertsResponse"></a>

### RaiseAlertsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [AlertResult](#shipping_api-AlertResult) | repeated | One result per alert sent, keyed by fulfillment order and type. Read already_open before reacting to anything: on a steady-state poller most entries are re-raises of a condition that has not gone away. |






<a name="shipping_api-RejectFulfillmentOrdersRequest"></a>

### RejectFulfillmentOrdersRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| rejections | [Rejection](#shipping_api-Rejection) | repeated | One entry per fulfillment order you are declining. Required and server-capped, like every batch on this contract. |






<a name="shipping_api-RejectFulfillmentOrdersResponse"></a>

### RejectFulfillmentOrdersResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [RejectionResult](#shipping_api-RejectionResult) | repeated | One result per rejection sent, matched on fulfillment_order_id. Anything that failed is still in ListNeedToShip on the next poll. |






<a name="shipping_api-Rejection"></a>

### Rejection



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | The work you are handing back, from ListNeedToShip. |
| reason | [RejectionReason](#shipping_api-RejectionReason) |  | Why you cannot fulfil it. Required — an unset reason is refused rather than stored, since the reason is the whole content of the record and OTHER already covers anything outside the list. |
| detail | [string](#string) |  | Free text shown to the user alongside the reason. |
| lines | [ShipmentLine](#shipping_api-ShipmentLine) | repeated | Not supported yet. Sending any line fails the entry rather than declining the whole order, which would hand back units you can still ship, silently. Omit it to decline the whole fulfillment order. |






<a name="shipping_api-RejectionResult"></a>

### RejectionResult



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | The rejection this answers, echoed back. |
| success | [bool](#bool) |  | True when the work is out of your queue, which includes a repeat of a rejection already recorded. False is a real failure and the work is still yours to ship or decline. |
| error_message | [string](#string) |  | Why it failed, in prose, for logs and support. Empty on success. The one worth handling rather than retrying is an order that has shipped, part-shipped or been cancelled, which can no longer be declined. |






<a name="shipping_api-ResolveAlertsRequest"></a>

### ResolveAlertsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| resolutions | [AlertResolution](#shipping_api-AlertResolution) | repeated | One entry per alert to clear. Required and server-capped. Only alerts this integration raised are in scope — naming one raised by Zentail or by another integration clears nothing. |






<a name="shipping_api-ResolveAlertsResponse"></a>

### ResolveAlertsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [AlertResolveResult](#shipping_api-AlertResolveResult) | repeated | One result per resolution sent, keyed by fulfillment order and type. Read already_resolved before reacting: nothing open to clear is the normal answer to a retry, and to a user having cleared it first. |






<a name="shipping_api-Shipment"></a>

### Shipment



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | Which fulfillment order this package is against. One package cannot span two fulfillment orders, so a consignment covering both halves of a split order is two Shipments. |
| external_shipment_id | [string](#string) |  | Your identifier for this package. Idempotency key: a repeat is a no-op. |
| carrier | [string](#string) |  | Who is carrying it, as you name them — free text, not a Zentail enum. Shown to the buyer and to support alongside the tracking number. |
| tracking_number | [string](#string) |  | Required. A package sent without one is refused rather than recorded, because Zentail drops an untracked package silently: accepting it would report success for a shipment that was never stored, leaving the units owed with nothing saying why. |
| tracking_url | [string](#string) |  | Not currently stored. Zentail tracks by carrier and tracking number and has nowhere to put a per-package URL, so anything sent here is accepted and discarded. Measured against sales-orders on 2026-09-21. |
| service_level | [string](#string) |  | Required, and not defaulted. The shipping speed you actually used, in your own vocabulary — Zentail will not substitute a value, because one guess for every fulfiller hides which of them reported nothing. Send a placeholder of your own if you have no real level to report. |
| shipped_ts | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  | When the package was handed to the carrier. Omit it and Zentail stamps the time the request arrived, which is the right behaviour for a confirmation sent immediately; send it when you are catching up on a backlog. |
| lines | [ShipmentLine](#shipping_api-ShipmentLine) | repeated | What is in the package. Omit to ship everything the fulfillment order still owes — the common case. Naming lines ships those quantities only; a line_item_id not on the order, a quantity below one, or quantities summing past what is owed fails this package and leaves the units owed. |
| shipping_cost | [Money](#shipping_api-Money) |  | What it cost the fulfiller to ship this package. Unset when they do not report one; unset means unknown and must not be read as zero, which would silently inflate margin. per ZEN-4054. |
| handling_cost | [Money](#shipping_api-Money) |  | A pick, pack or handling fee the fulfiller charges for this package, kept separate from postage rather than folded into it. Folding them loses the distinction permanently: postage is the carrier&#39;s price and varies by destination and weight, while a handling fee is the fulfiller&#39;s own and is usually flat, so a margin question about one cannot be answered from their sum. Unset means unknown, on the same terms as shipping_cost. per ZEN-4054. |






<a name="shipping_api-ShipmentLine"></a>

### ShipmentLine



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| line_item_id | [string](#string) |  | Which line of the fulfillment order, echoed from FulfillmentOrderLine. A SKU is not accepted here: one SKU can appear on two lines. |
| quantity | [int32](#int32) |  | How many units of that line are in this package. At least one, and never more than the line still owes — across the whole request, so naming one line twice is summed before it is checked. |






<a name="shipping_api-ShipmentResult"></a>

### ShipmentResult



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | The fulfillment order this package was against, echoed back. |
| external_shipment_id | [string](#string) |  | Your own id for the package, echoed back. This is what to match a result on: one call may carry several packages for the same fulfillment order. |
| success | [bool](#bool) |  | True when the package is recorded, including when it already was — a replay reports success with already_recorded set. False is always a real failure, and the units it covered are still owed. |
| error_message | [string](#string) |  | Why it failed, in prose, for logs and support. Empty on success. No error code accompanies it, so log the string rather than branching on it. |
| already_recorded | [bool](#bool) |  | True when this shipment was already recorded. Treat as success. |






<a name="shipping_api-WarehouseStatusRequest"></a>

### WarehouseStatusRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| warehouse_unique_id | [string](#string) |  | The warehouse being asked about, named with the integration&#39;s own identifier — the same value that appears on a fulfillment order. |






<a name="shipping_api-WarehouseStatusResponse"></a>

### WarehouseStatusResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| checks | [Check](#shipping_api-Check) | repeated | Everything the integration knows about this warehouse&#39;s health, one entry per condition. Leave `source` and `warehouse_unique_id` unset; Zentail overwrites both as it folds these into IntegrationStatus. An empty list means nothing to report, which Zentail renders as healthy. |





 


<a name="shipping_api-AlertType"></a>

### AlertType
AlertType maps to Zentail&#39;s customer-order alert types. Only the types a
fulfillment integration can legitimately raise are exposed.

| Name | Number | Description |
| ---- | ------ | ----------- |
| ALERT_TYPE_UNSPECIFIED | 0 | Refused. Every alert names a type. |
| ALERT_TYPE_FULFILL | 1 | Something is blocking fulfilment but you have not given up on it. If you have, reject instead. |
| ALERT_TYPE_LATE_SHIPMENT | 2 | Will miss, or has missed, the ship-by deadline. |
| ALERT_TYPE_LOST | 3 | Shipped but the package is lost in transit. |
| ALERT_TYPE_OTHER | 4 | Anything else worth telling a user about. Put the specifics in `message` — it is the only thing that reaches a person. |



<a name="shipping_api-CancellationReason"></a>

### CancellationReason


| Name | Number | Description |
| ---- | ------ | ----------- |
| CANCELLATION_REASON_UNSPECIFIED | 0 | Zentail always sets a reason. Read this as one it could not classify, not as an absent cancellation. |
| CANCELLATION_REASON_REROUTED | 1 | Re-routed to a different warehouse. |
| CANCELLATION_REASON_BUYER_CANCELLED | 2 | Cancelled by the buyer. |
| CANCELLATION_REASON_CHANNEL_CANCELLED | 3 | Cancelled by the sales channel. |
| CANCELLATION_REASON_MERCHANT_CANCELLED | 4 | Cancelled by a Zentail user. |



<a name="shipping_api-CheckSource"></a>

### CheckSource


| Name | Number | Description |
| ---- | ------ | ----------- |
| CHECK_SOURCE_UNSPECIFIED | 0 | Not set. Zentail fills this in on every check it returns, so this value only appears on a check an integration sent and Zentail has not folded in. |
| CHECK_SOURCE_ZENTAIL | 1 | Zentail observed this from the outside. |
| CHECK_SOURCE_INTEGRATION | 2 | The integration reported this about itself. |



<a name="shipping_api-CheckState"></a>

### CheckState


| Name | Number | Description |
| ---- | ------ | ----------- |
| CHECK_STATE_UNSPECIFIED | 0 | Zentail never sends this. Treat it as a check you cannot interpret rather than as a pass. |
| CHECK_STATE_PASS | 1 | Healthy, nothing to do. |
| CHECK_STATE_WARN | 2 | Working, but heading somewhere bad — a queue draining slower than it fills, a credential close to expiring. Worth looking at before it becomes a FAIL. |
| CHECK_STATE_FAIL | 3 | Broken now. Orders this check covers are not moving until it clears. |



<a name="shipping_api-FulfillmentOrderStatus"></a>

### FulfillmentOrderStatus
Status is informational. Do not drive behaviour from it — the queues already
say what Zentail wants done, and a status left over from a previous poll is
how an integration ends up shipping work it no longer owes.

| Name | Number | Description |
| ---- | ------ | ----------- |
| FULFILLMENT_ORDER_STATUS_UNSPECIFIED | 0 | Not a state Zentail sends. Read it as a status you cannot interpret. |
| FULFILLMENT_ORDER_STATUS_NEW | 1 | Routed to you, not yet acknowledged. |
| FULFILLMENT_ORDER_STATUS_ACCEPTED | 2 | Acknowledged and awaiting shipment. |
| FULFILLMENT_ORDER_STATUS_PARTIALLY_SHIPPED | 3 | Some units shipped and some are still owed; the remainder stays in ListNeedToShip. |
| FULFILLMENT_ORDER_STATUS_SHIPPED | 4 | Every routed unit shipped. Nothing further is owed. |
| FULFILLMENT_ORDER_STATUS_CANCELLED | 5 | Zentail pulled the whole thing back. |
| FULFILLMENT_ORDER_STATUS_PARTIALLY_CANCELLED | 6 | Part was pulled back. Ship whatever ListNeedToShip still shows. |
| FULFILLMENT_ORDER_STATUS_REJECTED | 7 | You declined it. Zentail reroutes or surfaces it. |



<a name="shipping_api-RejectionReason"></a>

### RejectionReason
RejectionReason is shown to the Zentail user deciding what to do next, so it
is required on every rejection. Pick the one that describes your side of the
problem; OTHER plus Rejection.detail is better than a near-miss.

| Name | Number | Description |
| ---- | ------ | ----------- |
| REJECTION_REASON_UNSPECIFIED | 0 | Refused. Send a real reason, or OTHER. |
| REJECTION_REASON_OUT_OF_STOCK | 1 | You hold the SKU but not enough of it to ship this work. |
| REJECTION_REASON_DAMAGED | 2 | The units are there but not shippable — damaged, expired, or failed a quality check. |
| REJECTION_REASON_UNDELIVERABLE_ADDRESS | 3 | The address will not deliver: your carrier refuses it, or it is incomplete. The likeliest reason a fulfillment order arrives with no ship_to at all. |
| REJECTION_REASON_SKU_NOT_FOUND | 4 | The SKU is not one you stock at all. Distinct from OUT_OF_STOCK: that one clears when stock arrives, this one needs someone to fix the catalogue or the routing. |
| REJECTION_REASON_OTHER | 5 | Anything else. Put the specifics in Rejection.detail — a user reads both. |


 

 


<a name="shipping_api-FulfillmentIntegrationService"></a>

### FulfillmentIntegrationService
FulfillmentIntegrationService is the contract a fulfillment integration —
a 3PL, a warehouse management system, or a storefront acting as a warehouse —
uses to do work on Zentail&#39;s behalf.

Every call is scoped to the integration resolved from the API token, and
through it to the warehouses bound to that integration. No request carries a
warehouse id or a company id; supplying one would let a caller ask about
warehouses that are not theirs.

**Two queues, and Zentail says what it wants.** Poll ListNeedToShip for work
to ship and ListNeedToCancel for work to pull back, then confirm each. There
is no change feed to diff and no status to interpret: if a fulfillment order
is in a queue, Zentail wants something done about it, and confirming is what
removes it. An integration that drains both queues is correct by
construction.

Neither queue takes a time filter. A queue is drained by confirming, never by
advancing a clock — a cursor would let an integration skip past work it
failed to finish, which is exactly the state the queue exists to represent.

**The unit of work is a fulfillment order, not an order.** One customer order
routed across two warehouses produces two fulfillment orders, and if both
warehouses are yours you receive both. Every write identifies its work by
`fulfillment_order_id`; `order_number` is carried for display and correlation
only, and is not unique to you.

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| ListNeedToShip | [ListNeedToShipRequest](#shipping_api-ListNeedToShipRequest) | [ListNeedToShipResponse](#shipping_api-ListNeedToShipResponse) | ListNeedToShip returns fulfillment orders you owe a shipment on: work routed to your warehouses and not yet shipped or cancelled.

New work and re-routed work look the same here, which is the point. A partial re-route reduces a line&#39;s quantity rather than removing the fulfillment order, so `lines` is authoritative on every read and must not be cached across polls.

An order leaves this queue when its lines are shipped, when Zentail cancels it, or when you reject it. Acknowledging does not remove it — you still owe the shipment. |
| ListNeedToCancel | [ListNeedToCancelRequest](#shipping_api-ListNeedToCancelRequest) | [ListNeedToCancelResponse](#shipping_api-ListNeedToCancelResponse) | ListNeedToCancel returns fulfillment orders you have acknowledged that Zentail now wants pulled back — re-routed to another warehouse, or cancelled by the buyer or the channel.

Only acknowledged work appears here. If you never told Zentail you had created it, there is nothing on your side to undo and the units simply leave ListNeedToShip.

A partial cancellation puts the same fulfillment order in **both** queues: cancel the quantities named here, ship what ListNeedToShip still shows. |
| GetFulfillmentOrder | [GetFulfillmentOrderRequest](#shipping_api-GetFulfillmentOrderRequest) | [FulfillmentOrder](#shipping_api-FulfillmentOrder) | GetFulfillmentOrder fetches one fulfillment order, whatever queue it is or is not in. For troubleshooting and for reconciling after a crash.

Pass exactly one of fulfillment_order_id or external_order_id. This is a point lookup, not a queue: an order it returns may still owe a shipment, and only ListNeedToShip says so. Polling it in place of the queues tells you what exists, never what Zentail wants done. |
| AcknowledgeFulfillmentOrders | [AcknowledgeFulfillmentOrdersRequest](#shipping_api-AcknowledgeFulfillmentOrdersRequest) | [AcknowledgeFulfillmentOrdersResponse](#shipping_api-AcknowledgeFulfillmentOrdersResponse) | AcknowledgeFulfillmentOrders records the identifier you gave the work on your own side. It does **not** drain ListNeedToShip — you still owe the shipment — and it is not required before shipping.

Two reasons to call it. It is how a crash between &#34;created it on my side&#34; and &#34;told Zentail&#34; becomes recoverable: the fulfillment order is still in ListNeedToShip, and the absence of your id there tells you to reconcile before creating a duplicate. And it is what puts the work into ListNeedToCancel if Zentail later needs it pulled back. |
| ConfirmShipments | [ConfirmShipmentsRequest](#shipping_api-ConfirmShipmentsRequest) | [ConfirmShipmentsResponse](#shipping_api-ConfirmShipmentsResponse) | ConfirmShipments reports packages that have shipped, draining the shipped quantities from ListNeedToShip.

Idempotent on external_shipment_id: replaying a shipment is a no-op, so a retry after a timeout can never double-ship. |
| ConfirmCancellations | [ConfirmCancellationsRequest](#shipping_api-ConfirmCancellationsRequest) | [ConfirmCancellationsResponse](#shipping_api-ConfirmCancellationsResponse) | ConfirmCancellations reports that you have pulled work back on your side, draining it from ListNeedToCancel.

Confirm only what you actually cancelled. If a unit has already shipped and cannot be recalled, say so with `already_shipped` rather than confirming — Zentail needs to know the difference to keep the customer order right. |
| RejectFulfillmentOrders | [RejectFulfillmentOrdersRequest](#shipping_api-RejectFulfillmentOrdersRequest) | [RejectFulfillmentOrdersResponse](#shipping_api-RejectFulfillmentOrdersResponse) | RejectFulfillmentOrders tells Zentail you cannot fulfil work it asked for — out of stock, damaged, address undeliverable. This is the other direction from ConfirmCancellations: there, Zentail asked; here, you are declining.

Zentail reroutes the work or surfaces it to the user, and it leaves ListNeedToShip. |
| RaiseAlerts | [RaiseAlertsRequest](#shipping_api-RaiseAlertsRequest) | [RaiseAlertsResponse](#shipping_api-RaiseAlertsResponse) | RaiseAlerts raises alerts against fulfillment orders, using the same alert model the rest of Zentail already shows on a customer order.

Idempotent by design: Zentail refuses a second open alert of the same type on the same order, so a poller can raise the same condition every pass without creating noise. No client-side &#34;have I already alerted?&#34; bookkeeping.

An alert is not a substitute for rejecting. Raise one to explain a delay; reject when you are not going to ship. |
| ResolveAlerts | [ResolveAlertsRequest](#shipping_api-ResolveAlertsRequest) | [ResolveAlertsResponse](#shipping_api-ResolveAlertsResponse) | ResolveAlerts resolves alerts **this integration raised**, recording why.

Scoping matters: an alert of the same type raised by Zentail itself, or by another integration, on the same order survives. Resolving by type alone would let one caller silently clear another&#39;s alerts. |
| IntegrationStatus | [IntegrationStatusRequest](#shipping_api-IntegrationStatusRequest) | [IntegrationStatusResponse](#shipping_api-IntegrationStatusResponse) | IntegrationStatus returns diagnostic checks for the caller&#39;s integration — whether warehouses are bound, whether either queue is being drained, and whether anything has been sitting in one for too long.

If the integration implements WarehouseService, Zentail also asks it about each warehouse and folds those checks in, so one call answers &#34;is this working&#34; from both sides. |


<a name="shipping_api-WarehouseService"></a>

### WarehouseService
WarehouseService is implemented by the integration, not by Zentail.

Same shape as listing&#39;s SalesChannelService in api-proto, and as the
WarehouseService in inventory-api-proto: the integration stands up this
service, Zentail dials it, and the answer folds into the surface an operator
already reads. Zentail can only observe an integration from the outside — it
knows the queue stopped draining, never why.

Deliberately separate from RaiseAlerts. An alert is a problem with one
fulfillment order, discovered at a moment Zentail cannot predict, so the
integration pushes it. A check is the integration&#39;s standing health, which is
only worth knowing when somebody asks — so Zentail pulls it, and a stale
answer is impossible.

Zentail calls this while serving IntegrationStatus, so it must be cheap and
must not call back into Zentail. If the call fails or times out, Zentail
reports that as a failed check rather than failing the status request: an
integration that cannot answer &#34;am I healthy&#34; has answered it.

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| WarehouseStatus | [WarehouseStatusRequest](#shipping_api-WarehouseStatusRequest) | [WarehouseStatusResponse](#shipping_api-WarehouseStatusResponse) | WarehouseStatus returns the integration&#39;s own diagnostic checks for one warehouse — the things only it can see, such as expiring credentials, a carrier account problem, or a location it can no longer reach.

Zentail calls this while serving IntegrationStatus, so answer from state you already hold and return quickly. Report a problem as a failing Check rather than as a gRPC error: an error is indistinguishable from the integration being unreachable, and loses whatever the check would have said. Returning no checks means &#34;nothing to report&#34;, which reads as healthy. |

 



## Scalar Value Types

| .proto Type | Notes | C++ | Java | Python | Go | C# | PHP | Ruby |
| ----------- | ----- | --- | ---- | ------ | -- | -- | --- | ---- |
| <a name="double" /> double |  | double | double | float | float64 | double | float | Float |
| <a name="float" /> float |  | float | float | float | float32 | float | float | Float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum or Fixnum (as required) |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="bool" /> bool |  | bool | boolean | boolean | bool | bool | boolean | TrueClass/FalseClass |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode | string | string | string | String (UTF-8) |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str | []byte | ByteString | string | String (ASCII-8BIT) |

