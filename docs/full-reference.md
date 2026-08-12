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
| acknowledgements | [Acknowledgement](#shipping_api-Acknowledgement) | repeated |  |






<a name="shipping_api-AcknowledgeFulfillmentOrdersResponse"></a>

### AcknowledgeFulfillmentOrdersResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [AcknowledgementResult](#shipping_api-AcknowledgementResult) | repeated |  |






<a name="shipping_api-Acknowledgement"></a>

### Acknowledgement



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| external_order_id | [string](#string) |  | Your own identifier for the work. Unique per integration — Zentail rejects a duplicate rather than recording it twice, which is what makes a retry safe. |






<a name="shipping_api-AcknowledgementResult"></a>

### AcknowledgementResult



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| success | [bool](#bool) |  |  |
| error_message | [string](#string) |  |  |
| already_acknowledged | [bool](#bool) |  | True when this external_order_id was already recorded — a replay, not a conflict. Treat as success. |






<a name="shipping_api-Address"></a>

### Address



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |
| company | [string](#string) |  |  |
| line_1 | [string](#string) |  |  |
| line_2 | [string](#string) |  |  |
| city | [string](#string) |  |  |
| region | [string](#string) |  |  |
| postal_code | [string](#string) |  |  |
| country | [string](#string) |  |  |
| phone | [string](#string) |  |  |
| email | [string](#string) |  |  |






<a name="shipping_api-Alert"></a>

### Alert



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| type | [AlertType](#shipping_api-AlertType) |  |  |
| message | [string](#string) |  | Shown to the user. Say what is wrong and what would fix it. |
| line_item_id | [string](#string) |  | Optional: scope the alert to one line. |






<a name="shipping_api-AlertResolution"></a>

### AlertResolution



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| type | [AlertType](#shipping_api-AlertType) |  |  |
| resolution | [string](#string) |  | Why it is resolved. Stored alongside the alert and shown to the user. |






<a name="shipping_api-AlertResolveResult"></a>

### AlertResolveResult
AlertResolveResult mirrors AlertResult but carries the resolve path&#39;s own
soft-success flag. AlertResult&#39;s already_open only makes sense when raising;
on a retried resolve it would be nonsense, leaving a caller that timed out
and retried with no honest way to read success.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| type | [AlertType](#shipping_api-AlertType) |  |  |
| success | [bool](#bool) |  |  |
| error_message | [string](#string) |  |  |
| already_resolved | [bool](#bool) |  | True when no open alert of this type remained, so nothing changed. Treat as success: it is what a retry after a timeout sees, and what a poller sees when a user resolved the alert by hand first. |






<a name="shipping_api-AlertResult"></a>

### AlertResult



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| type | [AlertType](#shipping_api-AlertType) |  |  |
| success | [bool](#bool) |  |  |
| error_message | [string](#string) |  |  |
| already_open | [bool](#bool) |  | True when an open alert of this type already existed, so nothing was created. Treat as success — this is the expected steady state for a poller re-raising a condition that has not gone away. |






<a name="shipping_api-CancellationConfirmation"></a>

### CancellationConfirmation



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| lines | [ShipmentLine](#shipping_api-ShipmentLine) | repeated | The quantities you actually pulled back. Omit to confirm everything ListNeedToCancel asked for. |
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
| sku | [string](#string) |  |  |
| quantity | [int32](#int32) |  | How many units to pull back — not the line&#39;s routed total. |






<a name="shipping_api-CancellationRequest"></a>

### CancellationRequest
CancellationRequest is Zentail asking for work back.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| external_order_id | [string](#string) |  | Your identifier, from the acknowledgement — the handle you need to find it on your own side. |
| order_number | [string](#string) |  |  |
| warehouse_unique_id | [string](#string) |  |  |
| reason | [CancellationReason](#shipping_api-CancellationReason) |  | Why, so it can be shown to a warehouse operator. |
| lines | [CancellationLine](#shipping_api-CancellationLine) | repeated | The quantities to pull back. A partial cancellation lists only some lines, or a lower quantity than the fulfillment order carries — ship the rest. |
| requested_ts | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  |  |






<a name="shipping_api-CancellationResult"></a>

### CancellationResult



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| success | [bool](#bool) |  |  |
| error_message | [string](#string) |  |  |
| already_recorded | [bool](#bool) |  | True when this cancellation was already recorded. Treat as success. |






<a name="shipping_api-Check"></a>

### Check



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  | Stable identifier, not prose — an operator or an alert matches on this, so it must not change when the wording does. Lower_snake_case by convention. |
| state | [CheckState](#shipping_api-CheckState) |  |  |
| message | [string](#string) |  | Prose for a human. Say what is wrong and what would fix it. |
| source | [CheckSource](#shipping_api-CheckSource) |  | Who observed this. Zentail sets it; an integration filling it in on a WarehouseStatus response has it overwritten with INTEGRATION. |
| warehouse_unique_id | [string](#string) |  | Which warehouse this check is about, when it is about one. Set by Zentail as it folds a WarehouseStatus response in, and on a check reporting that a warehouse could not be reached.

Without it the fold is lossy: two warehouses returning the same check name are indistinguishable, and anything matching on name alone cannot say which one is broken. Empty on checks about the integration as a whole. |






<a name="shipping_api-ConfirmCancellationsRequest"></a>

### ConfirmCancellationsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| confirmations | [CancellationConfirmation](#shipping_api-CancellationConfirmation) | repeated |  |






<a name="shipping_api-ConfirmCancellationsResponse"></a>

### ConfirmCancellationsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [CancellationResult](#shipping_api-CancellationResult) | repeated |  |






<a name="shipping_api-ConfirmShipmentsRequest"></a>

### ConfirmShipmentsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| shipments | [Shipment](#shipping_api-Shipment) | repeated |  |






<a name="shipping_api-ConfirmShipmentsResponse"></a>

### ConfirmShipmentsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [ShipmentResult](#shipping_api-ShipmentResult) | repeated |  |






<a name="shipping_api-FulfillmentOrder"></a>

### FulfillmentOrder
FulfillmentOrder is a lean view of the work: only the lines routed to one of
the caller&#39;s warehouses, with the quantities routed there.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  | Zentail&#39;s identifier for this unit of work, and the handle for every write in this API. Stable for the life of the fulfillment order. |
| order_number | [string](#string) |  | The customer order this came from. For display and correlation only — it is not unique to you, and is not accepted as an identifier on any write. |
| external_order_id | [string](#string) |  | Set once you have acknowledged it. |
| status | [FulfillmentOrderStatus](#shipping_api-FulfillmentOrderStatus) |  |  |
| warehouse_unique_id | [string](#string) |  | The warehouse these lines are routed to, in your own namespace. |
| ordered_ts | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  |  |
| assigned_ts | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  | When this work was routed to your warehouse. |
| last_updated_ts | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  |  |
| ship_by_ts | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  | Deadline for handing the package to the carrier, when the channel sets one. |
| ship_to | [Address](#shipping_api-Address) |  |  |
| requested_service_level | [string](#string) |  |  |
| buyer_name | [string](#string) |  |  |
| gift_message | [string](#string) |  |  |
| lines | [FulfillmentOrderLine](#shipping_api-FulfillmentOrderLine) | repeated | Authoritative on every read. Quantities change when Zentail re-routes work in or out of this warehouse, so never cache them across polls. |
| open_alerts | [Alert](#shipping_api-Alert) | repeated | Alerts you currently have open on this fulfillment order. Returned so a poller can see what it has already raised without keeping its own record — the same reason Zentail holds external_order_id. |






<a name="shipping_api-FulfillmentOrderLine"></a>

### FulfillmentOrderLine



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| line_item_id | [string](#string) |  | Stable per line; echo it back on shipment, cancellation and rejection. |
| sku | [string](#string) |  |  |
| title | [string](#string) |  |  |
| quantity | [int32](#int32) |  | Routed to your warehouse, not the customer order&#39;s total. |
| shipped_quantity | [int32](#int32) |  |  |
| cancelled_quantity | [int32](#int32) |  |  |






<a name="shipping_api-GetFulfillmentOrderRequest"></a>

### GetFulfillmentOrderRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| external_order_id | [string](#string) |  |  |






<a name="shipping_api-IntegrationStatusRequest"></a>

### IntegrationStatusRequest







<a name="shipping_api-IntegrationStatusResponse"></a>

### IntegrationStatusResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| checks | [Check](#shipping_api-Check) | repeated |  |






<a name="shipping_api-ListNeedToCancelRequest"></a>

### ListNeedToCancelRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| cursor | [string](#string) |  |  |
| page_size | [int32](#int32) |  |  |






<a name="shipping_api-ListNeedToCancelResponse"></a>

### ListNeedToCancelResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| cancellations | [CancellationRequest](#shipping_api-CancellationRequest) | repeated |  |
| next_cursor | [string](#string) |  |  |






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
| orders | [FulfillmentOrder](#shipping_api-FulfillmentOrder) | repeated |  |
| next_cursor | [string](#string) |  | Empty when the page is the last one. |






<a name="shipping_api-RaiseAlertsRequest"></a>

### RaiseAlertsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| alerts | [Alert](#shipping_api-Alert) | repeated |  |






<a name="shipping_api-RaiseAlertsResponse"></a>

### RaiseAlertsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [AlertResult](#shipping_api-AlertResult) | repeated |  |






<a name="shipping_api-RejectFulfillmentOrdersRequest"></a>

### RejectFulfillmentOrdersRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| rejections | [Rejection](#shipping_api-Rejection) | repeated |  |






<a name="shipping_api-RejectFulfillmentOrdersResponse"></a>

### RejectFulfillmentOrdersResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [RejectionResult](#shipping_api-RejectionResult) | repeated |  |






<a name="shipping_api-Rejection"></a>

### Rejection



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| reason | [RejectionReason](#shipping_api-RejectionReason) |  |  |
| detail | [string](#string) |  | Free text shown to the user alongside the reason. |
| lines | [ShipmentLine](#shipping_api-ShipmentLine) | repeated | Omit to reject the whole fulfillment order. Naming lines rejects only those quantities; the rest stays in ListNeedToShip. |






<a name="shipping_api-RejectionResult"></a>

### RejectionResult



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| success | [bool](#bool) |  |  |
| error_message | [string](#string) |  |  |






<a name="shipping_api-ResolveAlertsRequest"></a>

### ResolveAlertsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| resolutions | [AlertResolution](#shipping_api-AlertResolution) | repeated |  |






<a name="shipping_api-ResolveAlertsResponse"></a>

### ResolveAlertsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [AlertResolveResult](#shipping_api-AlertResolveResult) | repeated |  |






<a name="shipping_api-Shipment"></a>

### Shipment



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| external_shipment_id | [string](#string) |  | Your identifier for this package. Idempotency key: a repeat is a no-op. |
| carrier | [string](#string) |  |  |
| tracking_number | [string](#string) |  |  |
| tracking_url | [string](#string) |  |  |
| service_level | [string](#string) |  |  |
| shipped_ts | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  |  |
| shipping_cost | [double](#double) |  |  |
| lines | [ShipmentLine](#shipping_api-ShipmentLine) | repeated |  |






<a name="shipping_api-ShipmentLine"></a>

### ShipmentLine



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| line_item_id | [string](#string) |  |  |
| quantity | [int32](#int32) |  |  |






<a name="shipping_api-ShipmentResult"></a>

### ShipmentResult



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fulfillment_order_id | [string](#string) |  |  |
| external_shipment_id | [string](#string) |  |  |
| success | [bool](#bool) |  |  |
| error_message | [string](#string) |  |  |
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
| checks | [Check](#shipping_api-Check) | repeated |  |





 


<a name="shipping_api-AlertType"></a>

### AlertType
AlertType maps to Zentail&#39;s customer-order alert types. Only the types a
fulfillment integration can legitimately raise are exposed.

| Name | Number | Description |
| ---- | ------ | ----------- |
| ALERT_TYPE_UNSPECIFIED | 0 |  |
| ALERT_TYPE_FULFILL | 1 | Something is blocking fulfilment but you have not given up on it. If you have, reject instead. |
| ALERT_TYPE_LATE_SHIPMENT | 2 | Will miss, or has missed, the ship-by deadline. |
| ALERT_TYPE_LOST | 3 | Shipped but the package is lost in transit. |
| ALERT_TYPE_OTHER | 4 |  |



<a name="shipping_api-CancellationReason"></a>

### CancellationReason


| Name | Number | Description |
| ---- | ------ | ----------- |
| CANCELLATION_REASON_UNSPECIFIED | 0 |  |
| CANCELLATION_REASON_REROUTED | 1 | Re-routed to a different warehouse. |
| CANCELLATION_REASON_BUYER_CANCELLED | 2 | Cancelled by the buyer. |
| CANCELLATION_REASON_CHANNEL_CANCELLED | 3 | Cancelled by the sales channel. |
| CANCELLATION_REASON_MERCHANT_CANCELLED | 4 | Cancelled by a Zentail user. |



<a name="shipping_api-CheckSource"></a>

### CheckSource


| Name | Number | Description |
| ---- | ------ | ----------- |
| CHECK_SOURCE_UNSPECIFIED | 0 |  |
| CHECK_SOURCE_ZENTAIL | 1 | Zentail observed this from the outside. |
| CHECK_SOURCE_INTEGRATION | 2 | The integration reported this about itself. |



<a name="shipping_api-CheckState"></a>

### CheckState


| Name | Number | Description |
| ---- | ------ | ----------- |
| CHECK_STATE_UNSPECIFIED | 0 |  |
| CHECK_STATE_PASS | 1 |  |
| CHECK_STATE_WARN | 2 |  |
| CHECK_STATE_FAIL | 3 |  |



<a name="shipping_api-FulfillmentOrderStatus"></a>

### FulfillmentOrderStatus
Status is informational. Do not drive behaviour from it — the queues already
say what Zentail wants done, and a status left over from a previous poll is
how an integration ends up shipping work it no longer owes.

| Name | Number | Description |
| ---- | ------ | ----------- |
| FULFILLMENT_ORDER_STATUS_UNSPECIFIED | 0 |  |
| FULFILLMENT_ORDER_STATUS_NEW | 1 | Routed to you, not yet acknowledged. |
| FULFILLMENT_ORDER_STATUS_ACCEPTED | 2 | Acknowledged and awaiting shipment. |
| FULFILLMENT_ORDER_STATUS_PARTIALLY_SHIPPED | 3 |  |
| FULFILLMENT_ORDER_STATUS_SHIPPED | 4 |  |
| FULFILLMENT_ORDER_STATUS_CANCELLED | 5 |  |
| FULFILLMENT_ORDER_STATUS_PARTIALLY_CANCELLED | 6 |  |
| FULFILLMENT_ORDER_STATUS_REJECTED | 7 | You declined it. Zentail reroutes or surfaces it. |



<a name="shipping_api-RejectionReason"></a>

### RejectionReason


| Name | Number | Description |
| ---- | ------ | ----------- |
| REJECTION_REASON_UNSPECIFIED | 0 |  |
| REJECTION_REASON_OUT_OF_STOCK | 1 |  |
| REJECTION_REASON_DAMAGED | 2 |  |
| REJECTION_REASON_UNDELIVERABLE_ADDRESS | 3 |  |
| REJECTION_REASON_SKU_NOT_FOUND | 4 |  |
| REJECTION_REASON_OTHER | 5 |  |


 

 


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
| GetFulfillmentOrder | [GetFulfillmentOrderRequest](#shipping_api-GetFulfillmentOrderRequest) | [FulfillmentOrder](#shipping_api-FulfillmentOrder) | GetFulfillmentOrder fetches one fulfillment order, whatever queue it is or is not in. For troubleshooting and for reconciling after a crash. |
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
| WarehouseStatus | [WarehouseStatusRequest](#shipping_api-WarehouseStatusRequest) | [WarehouseStatusResponse](#shipping_api-WarehouseStatusResponse) | WarehouseStatus returns the integration&#39;s own diagnostic checks for one warehouse — the things only it can see, such as expiring credentials, a carrier account problem, or a location it can no longer reach. |

 



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

